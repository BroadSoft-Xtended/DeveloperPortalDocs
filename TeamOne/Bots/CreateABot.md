# How to use the Team-One RTM API

<div style="border-bottom:1px solid #ddd; margin-top:-0.4em; margin-bottom:1em; padding-bottom: 1.4em">
  <div style="float:right;text-align:right;font-size:80%;max-width:33%;">
  Revision 1.2.1<br>Last modified: 4 October 2016</div>
  &nbsp;
  <div style="font-size:60%; float:left; max-width:66%;"><i>This document is proprietary and confidential. No part of this document may be disclosed in any manner to a third party without prior written consent from BroadSoft.</i>
  </div>
</div>

  - [Step 0. Obtain an access token.](#step-0-obtain-an-access-token)
  - [Step 1. Connect to the RTM API.](#step-1-connect-to-the-rtm-api)
    - [Event Filtering Parameters](#event-filtering-parameters)
  - [Step 2. Send and receive RTM API messages.](#step-2-send-and-receive-rtm-api-messages)
      - [Chat Events](#chat-events)
        - [Sending Chat Messages](#sending-chat-messages)
        - [Receiving Chat Messages](#receiving-chat-messages)
          - [Chat Message Added](#chat-message-added)
          - [Chat Message Changed](#chat-message-changed)
          - [Chat Message Deleted](#chat-message-deleted)
      - [Typing Events](#typing-events)
        - [Sending Typing Events](#sending-typing-events)
        - [Receiving Typing Events](#receiving-typing-events)
      - [Note Events](#note-events)
        - [Sending Notes](#sending-notes)
        - [Receiving Notes](#receiving-notes)
          - [Note Added](#note-added)
          - [Note Changed](#note-changed)
          - [Note Deleted](#note-deleted)
      - [REST API Tunneling](#rest-api-tunneling)
      - [Ping/Pong](#pingpong)
      - [Error Handling](#error-handling)
  - [Step 3. Disconnect](#step-3-disconnect)
