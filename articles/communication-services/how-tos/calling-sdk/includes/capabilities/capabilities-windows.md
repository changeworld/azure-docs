---
author: DaybreakQuip
ms.service: azure-communication-services
ms.topic: include
ms.date: 06/20/2025
ms.author: zehangzheng
---

[!INCLUDE [Install SDK](../install-sdk/install-sdk-windows.md)]

The ability to view capabilities is an extended feature of the core `Call` API. It enables you to obtain the capabilities of the local participant in the current call.

The feature enables you to register for an event listener to listen for capability changes.

To use the Capabilities call feature for Windows, the first step is to obtain the Capabilities feature API object:

## Get the capabilities feature

```C#
private CapabilitiesCallFeature capabilitiesCallFeature;
capabilitiesCallFeature = call.Features.Capabilities;
```

## Get the capabilities of the local participant

The capabilities object has the capabilities of the local participants and is of type `ParticipantCapability`. Properties of capabilities include:

- *isAllowed* indicates if a capability can be used.
- *reason* indicates capability resolution reason.

```C#
var capabilities = capabilitiesCallFeature.Capabilities;
```

## Subscribe to `capabilitiesChanged` event

```C#
capabilitiesCallFeature.CapabilitiesChanged += Call__OnCapabilitiesChangedAsync;

private async void Call__OnCapabilitiesChangedAsync(object sender, CapabilitiesChangedEventArgs args)
{
    Trace.WriteLine(args.Reason.ToString());
    foreach (var capability in args.ChangedCapabilities)
    {
        //Prints out capability kind and resolution reason in console
        Trace.WriteLine(capability.Kind.ToString() + " is " + capability.Reason.ToString());
    }
}
```

## Capabilities Exposed

- *TurnVideoOn*: Ability to turn on video
- *UnmuteMicrophone*: Ability to unmute microphone
- *ShareScreen*: Ability to share screen
- *RemoveParticipant*: Ability to remove a participant
- *HangUpForEveryone*: Ability to hang up for everyone
- *AddCommunicationUser*: Ability to add a communication user
- *AddTeamsUser*: Ability to add Teams User
- *AddPhoneNumber*: Ability to add phone number
- *ManageLobby*: Ability to manage lobby
- *SpotlightParticipant*: Ability to spotlight Participant
- *RemoveParticipantSpotlight*: Ability to remove Participant spotlight
- *BlurBackground*: Ability to blur background
- *CustomBackground*: Ability to apply a custom background
- *StartLiveCaptions*: Ability to start live captions
- *RaiseHand*: Ability to raise hand
- *MuteOthers*: Ability to soft mute remote participants in the meeting 
