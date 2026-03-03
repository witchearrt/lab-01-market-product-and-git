# Product Architecture Description

## Product Choice

- **Product name:** Telegram
- **Link to the product's website:** https://telegram.org
- **Short description:** Telegram is a cloud-based messaging app focused on security and speed. It supports text, voice, and video messages, as well as file sharing between users and groups.

## Main components

![Telegram Component Diagram](../docs/diagrams/out/telegram/component-diagram/Component%20Diagram.svg)

**PlantUML code:** [Telegram Component Diagram Code](../docs/diagrams/src/telegram/component-diagram.puml)

### Component Descriptions

1. **Client Application** — user-facing application (mobile, desktop, or web) that provides the interface for sending/receiving messages, managing chats, and configuring settings.

2. **Message Router** — server-side component that routes incoming messages between clients, determining recipients and ensuring real-time delivery.

3. **Cloud Storage** — distributed storage for messages, media files, and user data, enabling synchronization across devices and access to chat history.

4. **Authentication Service** — service managing user identification, including registration, phone number login, and session management.

5. **Encryption Module** — component responsible for encrypting data in transit (MTProto) and managing encryption keys for secret chats.

## Data flow

![Telegram Sequence Diagram](../docs/diagrams/out/telegram/sequence-diagram/Sequence%20Diagram.svg)

**PlantUML code:** [Telegram Sequence Diagram Code](../docs/diagrams/src/telegram/sequence-diagram.puml)

### Flow: Message Sending

In this flow, a user sends a message through the client application. The client encrypts the message via the Encryption Module and sends it to the Message Router. The router identifies the recipient, stores a copy in Cloud Storage, and forwards the message to the recipient's client. The recipient decrypts and displays the message.

**Components and data exchanged:**
- **Client → Encryption Module:** encrypted message
- **Client → Message Router:** metadata (sender, recipient, timestamp)
- **Message Router → Cloud Storage:** message for storage
- **Message Router → Recipient Client:** delivered message

## Deployment

![Telegram Deployment Diagram](../docs/diagrams/out/telegram/deployment-diagram/Deployment%20Diagram.svg)

**PlantUML code:** [Telegram Deployment Diagram Code](../docs/diagrams/src/telegram/deployment-diagram.puml)

### Deployment Description

- **Client Devices:** mobile devices (iOS/Android), desktop applications (Windows/macOS/Linux), and web clients connect to Telegram servers via the internet.
- **Application Servers:** application servers are hosted in distributed data centers and handle message routing, authentication, and business logic.
- **Storage Servers:** dedicated servers for Cloud Storage provide reliable storage of messages and media files with data replication.
- **CDN:** content delivery network is used for fast transfer of large files and media worldwide.

## Assumptions

1. I assume the Cloud Storage implements deduplication to optimize storage costs for shared media files (e.g., when the same image is sent to multiple users).

2. I assume the Message Router uses in-memory caching of active sessions to ensure low latency for real-time message delivery.

## Open questions

1. How exactly does the distributed synchronization mechanism work between multiple data centers when accessing the same chat from different devices simultaneously?

2. What specific scaling strategies are used to handle peak loads during mass events or news spikes?
