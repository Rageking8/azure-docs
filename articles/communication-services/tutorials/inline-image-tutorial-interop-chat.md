---
title: Enable inline image using UI Library in Teams Interoperability Chat
titleSuffix: An Azure Communication Services tutorial
description: Learn how to use the UI Library to enable inline image support in Teams Interoperability Chat
author: jpeng-ms
services: azure-communication-services
ms.author: jopeng
ms.date: 08/03/2023
ms.topic: tutorial
ms.service: azure-communication-services
ms.subservice: chat
---

# Enable inline image using UI Library in Teams Interoperability Chat

In a Teams Interoperability Chat ("Interop Chat"), we can enable Azure Communication Service end users to receive inline images sent by Teams users. Additionally, when rich text editor is enabled, Azure Communication Service end users can send inline images to Teams users. Refer to [UI Library Use Cases](../concepts/ui-library/ui-library-use-cases.md) to learn more.

> [!IMPORTANT]
>
> Receiving inline images feature comes with the CallWithChat Composite without additional setups. 
> Sending inline images feature can be enabled by set `richTextEditor` to true under the CallWithChatCompositeOptions.

> [!IMPORTANT]
> The sending inline image feature of Azure Communication Services is currently in preview.
>
> Preview APIs and SDKs are provided without a service-level agreement. We recommend that you don't use them for production workloads. Some features might not be supported, or they might have constrained capabilities.
>
> For more information, review [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## Download code

Access the code for this tutorial on [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/ui-library-quickstart-teams-interop-meeting-chat).

## Prerequisites

- An Azure account with an active subscription. For details, see [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Visual Studio Code](https://code.visualstudio.com/) on one of the [supported platforms](https://code.visualstudio.com/docs/supporting/requirements#_platforms).
- [Node.js](https://nodejs.org/), Active LTS and Maintenance LTS versions. Use the `node --version` command to check your version.
- An active Communication Services resource and connection string. [Create a Communication Services resource](../quickstarts/create-communication-resource.md).
- Using the UI library version [1.15.0](https://www.npmjs.com/package/@azure/communication-react/v/1.15.0) or the latest for receiving inline images. Using the UI library version [1.19.0-beta.1](https://www.npmjs.com/package/@azure/communication-react/v/1.19.0-beta.1) or the latest beta version for sending inline images.
- Have a Teams meeting created and the meeting link ready.
- Be familiar with how [ChatWithChat Composite](https://azure.github.io/communication-ui-library/?path=/docs/composites-call-with-chat-basicexample--basic-example) works.


## Background

First of all, we need to understand that Teams Interop Chat has to be part of a Teams meeting currently. When the Teams user creates an online meeting, a chat thread would be created and associated with the meeting. To enable the Azure Communication Service end user joining the chat and starting to send/receive messages, a meeting participant (a Teams user) would need to admit them to the call first. Otherwise, they don't have access to the chat.

Once the Azure Communication Service end user is admitted to the call, they would be able to start to chat with other participants on the call. In this tutorial, we're checking out how inline image works in Interop chat.

## Overview

As mentioned previously, since we need to join a Teams meeting first, we need to use the ChatWithChat Composite from the UI library. 

Let's follow the basic example from the [storybook page](https://azure.github.io/communication-ui-library/?path=/docs/composites-call-with-chat-basicexample--basic-example) to create a ChatWithChat Composite.

From the sample code, it needs `CallWithChatExampleProps`, which is defined as the following code snippet:

```js
export type CallWithChatExampleProps = {
  // Props needed for the construction of the CallWithChatAdapter
  userId: CommunicationUserIdentifier;
  token: string;
  displayName: string;
  endpointUrl: string;
  locator: TeamsMeetingLinkLocator | CallAndChatLocator;

  // Props to customize the CallWithChatComposite experience
  fluentTheme?: PartialTheme | Theme;
  compositeOptions?: CallWithChatCompositeOptions;
  callInvitationURL?: string;
};

```
There no specific setup needed to enable receiving inline images. However, to be able to send inline images, `richTextEditor` function need to be enabled through the `CallWithChatExampleProps`. Here's a code snippet on how to enable it:
```js
<CallWithChatExperience
  // ...any other call with chat props
  compositeOptions={{ richTextEditor: true }}
/>

```


To be able to start the Composite for meeting chat, we need to pass `TeamsMeetingLinkLocator`, which looks like this:

```js
{ "meetingLink": "<TEAMS_MEETING_LINK>" }
```

This is all you need - and there's no other setup needed. 


## Run the code

Let's run `npm run start` then you should be able to access our sample app via `localhost:3000` like the following screenshot: 

![Screenshot of a Azure Communication Services UI library.](./media/inline-image-tutorial-interop-chat-0.png "Screenshot of a Azure Communication Services UI library.")

Simply click on the chat button located in the bottom to reveal the chat panel and now if Teams user sends an image, you should see something like the following screenshot:

!["Screenshot of a Teams client sending 2 inline images."](./media/inline-image-tutorial-interop-chat-1.png "Screenshot of a Teams client sending 2 inline images.")

![Screenshot of Azure Communication Services UI library receiving two inline images.](./media/inline-image-tutorial-interop-chat-2.png "Screenshot of Azure Communication Services UI library receiving 2 inline images.")


When sending inline images is enabled, you should see something like the following screenshot:
![Screenshot of Azure Communication Services UI library sending two inline images and editing messages.](./media/inline-image-tutorial-interop-chat-3.png "Screenshot of Azure Communication Services UI library sending 2 inline images and editing messages.")

!["Screenshot of a Teams client receiving 2 inline images."](./media/inline-image-tutorial-interop-chat-4.png "Screenshot of a Teams client receiving 2 inline images.")


## Known Issues

* The UI library might not support certain GIF images at this time. The user might receive a static image instead.
* The Web UI library doesn't support Clips (short videos) sent by the Teams users at this time.
* For certain Android devices, pasting of a single image is only supported when long pressing on the rich text editor and choosing
paste. Selecting from the clipboard view from keyboard may not be supported.


## Next steps

> [!div class="nextstepaction"]
> [Check the rest of the UI Library](https://azure.github.io/communication-ui-library/)

You may also want to:

- [Check UI Library use cases](../concepts/ui-library/ui-library-use-cases.md)
- [Add chat to your app](../quickstarts/chat/get-started.md)
- [Creating user access tokens](../quickstarts/identity/access-tokens.md)
- [Learn about client and server architecture](../concepts/client-and-server-architecture.md)
- [Learn about authentication](../concepts/authentication.md)
- [Add file sharing with UI Library in Azure Communication Service Chat](./file-sharing-tutorial-acs-chat.md)
- [Add file sharing with UI Library in Teams Interoperability Chat](./file-sharing-tutorial-interop-chat.md)
