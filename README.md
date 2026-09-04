# Digital Campus Card - Bildungscampus

## Overview
The **Digital Campus Card** is an initiative to modernize the student and staff experience at the Bildungscampus. Currently, access to facilities, library services, and payments relies on a physical NFC smart card. 

This project aims to digitize the physical card, utilizing modern smartphone NFC capabilities (Host Card Emulation) so users can simply tap their phones instead of carrying a physical card.

Physical cards are easily lost, forgotten, or damaged. Replacing them costs time and money.

Since almost every student and staff member carries a smartphone with an NFC chip, we can integrations with Host Card Emulation to securely mirror the physical card's credentials on a mobile device.

## Features (Planned)
*   **Smartphone NFC Emulation:** Use your phone to tap into buildings, the library, and parking facilities.
*   **Digital Payments:** Tap to pay at the Mensa and cafeterias among others.
*   **Balance & Transaction History:** Instantly check your campus card balance and recent transactions directly in the app.
*   **Secure & Private:** Utilizes secure element and encrypted tokenization to ensure your data and funds are safe.

## Tech Stack
*   **Frontend / Mobile App:** Android (Java)
*   **Backend / API:** Java (JDK 17+) / javax.smartcardio
*   **Mock Database:** JSON
