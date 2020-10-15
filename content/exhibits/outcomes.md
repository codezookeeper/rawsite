---
title: "Outcomes"
date: 2020-09-28T10:59:31-04:00
tags: ["Java", "Android", "Kotlin"]
draft: true
---

{{< figure src="https://www.google.com/url?sa=i&url=https%3A%2F%2Fforums.fast.ai%2Ft%2Frop-railway-oriented-programming-in-swift%2F44428&psig=AOvVaw1qCkMTyrqrUwB26kX76R-E&ust=1601391614563000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCMC9uuSOjOwCFQAAAAAdAAAAABAI" caption="" >}}

## Classification
> Class: [Officium]  
> Order: [Probatis]  
> Family: [Persistence]  
> Genus: []  
> Species: []  

## Related
"Kotlin version"[1.4.10]  

## Concerning Functions

It can be assumed most parents try thier best to instill in their children the tennents of ethical conduct such as they see them. One of those is perhaps "Honesty is the Best Policy", and can be applied not only in one's daily exploits in life, but throughout the development kingdom as well. If you've not explored our exhibit on [Honest Functions]("#index.html/honest-functions"), perhpas you might backtrack and have a look to further understand the philosophies surrounding this particular order of functions.

It's very common to observe what we like to call "dishonest functions" (Officium Res Turpis) in the wild. Here we have a typical example of function passing a value with the implication of persiting the passed value somewhere. However, it is not clear that once the function is called, the value has been persisted in any fashion. Indeed, the caller can only assume the the operation was successful. We call this function dishonest. However, it's not the function's fault per se and further, it's not that it's nefarious in nature. Rather, it's simply in need of taming. With a little refactoring, we can corall this wild function and help it become mature, well structured code.

## Wild sample

```kotlin

make this a web call instead
fun setDeviceToCloud(device: UserDevice?) {
        device?.let {
            userDeviceApi.sendUserDevice(it)
        }
}
```

```kotlin

setDeviceToCloud(device)

navigateToViewThatDisplaysDeviceInformation()
                    
```

## Wild observations
Given that `device` means an optional `UserDevice` object that has been generated after attaining a Bluetooth device from some BLE Scan, we assume the `setDeviceToCloud(device)` function will make a web call and send the relevant data to some far-off database for persistence.

There are two direct problems with this code. First, note the proceedural nature of the call site. It assumes that setting the device is successful and immediatly proceceeds to a navigation call that might require the device being properly set and would thusly crash or enter an undesired state. Secondly, and related to the first problem, the wild function accepts an optional value. This in and of itself is not necessarily an issue if handled appropriately but if the value is null, the function will not perform the expected behavior. Ultimately, the navigation method should not be called if the device is null or the `setDeviceToCloud(device)` method is unsuccessful. As it is, the function does not enforce this requirement. 

In order to tame this code, we need to implmement some [Railway-Oriented Programming](https://proandroiddev.com/railway-oriented-programming-in-kotlin-f1bceed399e5). 

## Tamed code

Here is an example of how we might implement a Railway-Oriented paradigm using Outcomes.

```kotlin 
sealed class Outcome<out T> {
    data class Success<out T>(val value: T) : Outcome<T>()
    data class Error(val error: Any) : Outcome<Nothing>()

    companion object {
        inline fun <T> tryBlock(block: () -> T): Outcome<T> = try {
            Success(block())
        } catch (e: Exception) {
            Error(e)
        }
    }
}
```

```kotlin
fun setDeviceToCloud(device: BluetoothDevice?): Outcome<Boolean> {

    //1. Let's handle the case where the object is null
    if(device == null) return Outcome.Error("Cannot send null object.")

    //2. With the null case out of the way, we can turn our attention to the result of the web call.
    return when(val status = userDeviceApi.sendUserDevice(device)) {
        is 200 -> Outcome.Success(status.body())
        else -> Outcome.Error(status.errorBody())
    }
}
```

Now in our call site: 

```kotlin
when(val outcome = setDeviceToCloud(device)){
    is Outcome.Success -> {
        navigateToViewThatDisplaysDeviceInformation()
    }
    is Outcome.Error -> {
        displayErrorMessage(outcome.error)
    }
}
```


## Domesticated observations
It's clear what to expect from the `setDeviceToCloud(device)` function now. This way, no matter the result, it's obvious what track the code should take. Railway-Oriented programming is a powerfull way to introduce a little functional programming into existing code. 

## Related exhibits

[something]  
[something else]  