---
layout: post
title:  Android RIL code review
category: technology 
---

#RIL Overview#

RIL is the abbreviation of "Radio Interface Layer". In the android software platform, the RIL sub system communicates with BaseBand chip downwards, and provides the android framework with phones' related services upwards, such as Call, sms, etc. Its location in android softeware  stack is shown in graph below:

![alt text](/images/notes/RIL-architecture.PNG"RIL-architecture.PNG")

In the lower layer, RIL communicates with Baseband chip via the linux virtual UART driver, we don't want to cover some details about the interactions between the UART driver and the real baseband chip, in this place, this serial port is on behalf of the baseband chip. In the upper layer, RIL communicates with the framework via socket. The java layer in framework is not our point, what we will talk about is the green part in above graph.

#The compoents in RIL#

The google company seprated the RIL into three parts in order to protect the IP of the OEM. These parts are: RIL daemon application(RILD), libril.so and reference-ril.so. The RILD and the libril.so reference each other in static linking, their implementations are open to us, but reference-ril.so is the reference implementation which is provided by Google to OEMs. Because of the diversity and the complexity of all kinds of the mobile network. The oem vendors have their own process routines in their baseband chip, they may don't want to open the source code, so they implement their own vendor-ril.so. In order to loose coupling between the private module and the open source module, RILD uses the dynamic loading method to communicate with vendor-ril.so. Their source codes are at:

    RILD--
		hardware/ril/rild

	libril--
		hardware/ril/libril   

Furthermore, the libril.so and the reference-ril.so have the different functions in RIL architecture, libril.so is used to handle I/O messages with Android Framework via socket. But reference-ril.so is responsible for communicating with the serial port. 

#Start from RILD#

The entire RIL system is initiated by the invoking RILD command from shell, so let's start with it.

There is a static structure object in rild.c:

{% highlight c %}
	static struct RIL_Env s_rilEnv = {
		RIL_onRequestComplete,
		RIL_onUnsolicitedResponse,
		RIL_requestTimedCallback,
	};
{% endhighlight %}


It includes three function pointers, their implementaions are at libril.so, and they will be used by vendor ril. On the contrary, there is another similar structure defined in Ril.h:

{% highlight c %}
	typedef struct {
		int version;        /* set to RIL_VERSION */
		RIL_RequestFunc onRequest;
		RIL_RadioStateRequest onStateRequest;
		RIL_Supports supports;
		RIL_Cancel onCancel;
		RIL_GetVersion getVersion;
	} RIL_RadioFunctions;
{% endhighlight %}

This structure includes five function pointers, their implementations are at vendor ril, they will be used by libril.so. So, vendor ril and libril.so invoke each other's function to implement the RIL system. By the way, libril.so and rild are connected by static linking. But android wants to hide the OEM vendor's implementation, vendor-ril.so is connected with rild by dynamic linking. So let's look at the code snippets of the dynamic loading of reference-ril.so:

{% highlight c %}
	dlHandle = dlopen(rilLibPath, RTLD_NOW);
    if (dlHandle == NULL) {
		fprintf(stderr, "dlopen failed: %s\n", dlerror());
		exit(-1);
	}

    RIL_startEventLoop();
    rilInit = (const RIL_RadioFunctions *(*)(const struct RIL_Env *, int, char **))dlsym(dlHandle, "RIL_Init");
    if (rilInit == NULL) {
		fprintf(stderr, "RIL_Init not defined or exported in %s\n", rilLibPath);
		exit(-1);
	}
{% endhighlight %}

As the code snippets show us, we get the path of the vendor ril from the configuration file, then invoke RIL_startEventLoop, its implementation is in libril.so, that the code below:

{% highlight c %}
	extern "C" void
	RIL_startEventLoop(void) {
		int ret;
		pthread_attr_t attr;
		
		/* spin up eventLoop thread and wait for it to get started */
		s_started = 0;
		pthread_mutex_lock(&s_startupMutex);
		
		pthread_attr_init (&attr);
		pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
		ret = pthread_create(&s_tid_dispatch, &attr, eventLoop, NULL);

		while (s_started == 0) {
				pthread_cond_wait(&s_startupCond, &s_startupMutex);
		}

		pthread_mutex_unlock(&s_startupMutex);

		if (ret < 0) {
				LOGE("Failed to create dispatch thread errno:%d", errno);
				return;
		}
	}
{% endhighlight %}

We will discuss this function's detail in next section, so let's continue to check the main function's rutine of RILD, after invoking RIL_startEventLoop, we get the function pointer of "RIL_Init" in vendor ril, then after checking the validation of the function pointer, we invoke it with main's input parameters:

{% highlight c %}
    rilArgv[0] = argv[0];
	
	funcs = rilInit(&s_rilEnv, argc, rilArgv);

	LOGD("RIL_register %d", cardNum);
	RIL_register(funcs,cardNum);
{% endhighlight %}

The RIL_Init implemented in vendor ril will do some low layer's jobs: 

* Set the RIL_RadioFunctions pointer to vendor ril, so vendor ril can call libril's functions.
* Launching the mainLoop thread to handle hardware , including initializing the UART, reading data from the UART. 
* Returning five function pointers that will be used in libril.so to RILD. 

We will talk about it with more details in later section. The last invoking is RIL_register, we transfer the previous function pointers into libril.so via this function. Inner the RIL_register, it connects the socket to communicate the framework, and launch the listenCallback event, it's like a engine can always trigger the I/O jobs. 

For a summay of the rild rutine, there is a graph below: 
![alt text](/images/notes/RIL_rutine.PNG"RIL_rutine.PNG")
