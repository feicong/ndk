android ndk jni module for use internal libs android from source code.

## usage
push code from github to your jni folder:
```
$ git clone https://github.com/feicong/ndk
```

apply these lines to your Android.mk file:
```
LOCAL_C_INCLUDES       	:= $(LOCAL_PATH)
include $(CLEAR_VARS)
LOCAL_MODULE 			:= android_runtime
LOCAL_SRC_FILES 		:= ndk/lib/$(TARGET_ARCH_ABI)/libandroid_runtime.so
include $(PREBUILT_SHARED_LIBRARY)
include $(CLEAR_VARS)

LOCAL_C_INCLUDES       	:= $(LOCAL_PATH)
include $(CLEAR_VARS)
LOCAL_MODULE 			:= binder
LOCAL_SRC_FILES 		:= ndk/lib/$(TARGET_ARCH_ABI)/libbinder.so
include $(PREBUILT_SHARED_LIBRARY)
include $(CLEAR_VARS)

LOCAL_C_INCLUDES       	:= $(LOCAL_PATH)
include $(CLEAR_VARS)
LOCAL_MODULE 			:= utils
LOCAL_SRC_FILES 		:= ndk/lib/$(TARGET_ARCH_ABI)/libutils.so
include $(PREBUILT_SHARED_LIBRARY)
include $(CLEAR_VARS)
```

now use it in your module:
```
LOCAL_C_INCLUDES 		:= $(LOCAL_PATH)/ndk/include
LOCAL_MODULE            := module1
LOCAL_SHARED_LIBRARIES	:= android_runtime binder utils
LOCAL_LDLIBS            := -ldl -llog
# LOCAL_LDFLAGS := -Wl,--unresolved-symbols=ignore-all
LOCAL_SRC_FILES         := txyservice/txyservice.cpp
LOCAL_CFLAGS 			:= -fvisibility=hidden #默认不导出
include $(BUILD_SHARED_LIBRARY)
```

add code to your Android Studio module file build.gradle:
```
    ......
    compileSdkVersion 21
    buildToolsVersion '25.0.2'
    defaultConfig {
        minSdkVersion 9
        targetSdkVersion 19

        externalNativeBuild {
            ndkBuild {
                abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a', 'x86'
                arguments "APP_STL=gnustl_static"
                arguments "ANDROID_PLATFORM=android-21"
                arguments "NDK_TOOLCHAIN_VERSION=clang"
                arguments "-j8"
                targets "module1", "module2", "module3"
                cppFlags "-DHAVE_SYS_UIO_H"
            }
        }
    ......
```

now, enjoy it !
