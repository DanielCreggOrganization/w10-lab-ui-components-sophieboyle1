# Ionic UI Components Lab: Essential Components

## Agenda
1. [Project Setup](#project-setup)
2. [Ion-Tabs: App Navigation](#1-ion-tabs-app-navigation)
3. [Ion-Card: Basic Content Display](#2-ion-card-basic-content-display)
4. [Ion-List: Interactive Lists](#3-ion-list-interactive-lists)
5. [Ion-Modal: Popup Dialogs](#4-ion-modal-popup-dialogs)
6. [Ion-Alert & Ion-Toast: User Notifications](#5-ion-alert--ion-toast-user-notifications)

## Overview
In this lab, you'll learn about essential Ionic UI components by implementing them one at a time. Each section includes a demonstration followed by a hands-on DIY task to enhance your understanding.

## Project Setup
Create a new project with tabs:
```bash
ionic start ui-components-demo tabs --type=angular
cd ui-components-demo
```

## 1. Ion-Tabs: App Navigation

### Introduction
Ion-Tabs provide a consistent way to organize and navigate between different sections of your app.

### Implementation
Update `src/app/tabs/tabs.page.ts`:
```typescript
import { Component, EnvironmentInjector, inject } from '@angular/core';
import { IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel } from '@ionic/angular/standalone';
import { addIcons } from 'ionicons';
import { home, person, notifications } from 'ionicons/icons';

@Component({
  selector: 'app-tabs',
  templateUrl: 'tabs.page.html',
  standalone: true,
  imports: [IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel],
})
export class TabsPage {
  notificationCount = 3;
  
  constructor() {
    addIcons({ home, person, notifications });
  }
}
```

Update `src/app/tabs/tabs.page.html`:
```html
<ion-tabs>
  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="tab1" href="/tabs/tab1">
      <ion-icon name="home"></ion-icon>
      <ion-label>Home</ion-label>
    </ion-tab-button>

    <ion-tab-button tab="tab2" href="/tabs/tab2">
      <ion-icon name="person"></ion-icon>
      <ion-label>Profile</ion-label>
    </ion-tab-button>

    <ion-tab-button tab="tab3" href="/tabs/tab3">
      <ion-icon name="notifications"></ion-icon>
      <ion-label>Notifications</ion-label>
      <ion-badge>{{notificationCount}}</ion-badge>
    </ion-tab-button>
  </ion-tab-bar>
</ion-tabs>
```

Add some style in `tabs.page.scss`:
```scss
ion-tab-button {
  --color-selected: #ff4081;
  
  &.tab-selected {
    ion-icon {
      transform: scale(1.1);
    }
  }
}
```

### DIY Task
1. Add a fourth tab for "Settings" with an appropriate icon
2. Add a custom animation when tabs are selected
3. Change the color scheme to use different colors for selected and unselected states

## 2. Ion-Card: Basic Content Display

### Introduction
Ion-Card is used to display content in a card format with various elements like headers, content, and actions.

### Implementation
Create a simple card in `tab1.page.html`:
```html
<ion-content>
  <ion-card>
    <img alt="card-image" src="https://picsum.photos/300/200"/>
    <ion-card-header>
      <ion-card-title>Card Title</ion-card-title>
      <ion-card-subtitle>Card Subtitle</ion-card-subtitle>
    </ion-card-header>

    <ion-card-content>
      Here's some content for the card. This is a basic example of an Ion-Card.
    </ion-card-content>

    <ion-button fill="clear">Action</ion-button>
  </ion-card>
</ion-content>
```

### DIY Task
1. Add multiple action buttons to the card
2. Implement a card with a different layout (e.g., horizontal instead of vertical)
3. Add hover effects to make the card interactive

## 3. Ion-List: Interactive Lists

### Introduction
Ion-Lists are perfect for displaying rows of information with various interactive features.

### Implementation
Add this to `tab2.page.html`:
```html
<ion-content>
  <ion-list>
    <ion-item>
      <ion-label>
        <h2>Item Title</h2>
        <p>Item Description</p>
      </ion-label>
      <ion-badge slot="end">New</ion-badge>
    </ion-item>

    <ion-item-sliding>
      <ion-item>
        <ion-label>Slidable Item</ion-label>
      </ion-item>
      
      <ion-item-options side="end">
        <ion-item-option color="primary">
          <ion-icon slot="icon-only" name="heart"></ion-icon>
        </ion-item-option>
        <ion-item-option color="danger">
          <ion-icon slot="icon-only" name="trash"></ion-icon>
        </ion-item-option>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
</ion-content>
```

### DIY Task
1. Add different types of items (with avatars, thumbnails, icons)
2. Implement sliding options on both sides of items
3. Add different interaction states (selected, disabled)

## 4. Ion-Modal: Popup Dialogs

### Introduction
Ion-Modal provides a way to show detailed content or forms in a popup overlay.

### Implementation
In `tab3.page.html`:
```html
<ion-content>
  <ion-button id="open-modal" expand="block">
    Open Modal
  </ion-button>

  <ion-modal trigger="open-modal">
    <ng-template>
      <ion-header>
        <ion-toolbar>
          <ion-title>Modal Title</ion-title>
          <ion-buttons slot="end">
            <ion-button onclick="modal.dismiss()">Close</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <h2>Modal Content</h2>
        <p>This is a basic modal example.</p>
      </ion-content>
    </ng-template>
  </ion-modal>
</ion-content>
```

### DIY Task
1. Create a modal with a form inside
2. Add animations to the modal enter/exit
3. Implement a half-sheet modal style

## 5. Ion-Alert & Ion-Toast: User Notifications

### Introduction
Alerts and toasts are essential for providing feedback to users.

### Implementation
Add to any page component:
```typescript
import { AlertController, ToastController } from '@ionic/angular/standalone';

export class YourPage {
  constructor(
    private alertController: AlertController,
    private toastController: ToastController
  ) {}

  async showAlert() {
    const alert = await this.alertController.create({
      header: 'Alert',
      subHeader: 'Important message',
      message: 'This is an alert message.',
      buttons: ['OK']
    });
    await alert.present();
  }

  async showToast() {
    const toast = await this.toastController.create({
      message: 'This is a toast message',
      duration: 2000,
      position: 'bottom'
    });
    await toast.present();
  }
}
```

### DIY Task
1. Create an alert with multiple buttons and different actions
2. Implement a toast with a custom close button
3. Add different positions and animations for the toast

## Additional Resources
- [Ionic UI Components Documentation](https://ionicframework.com/docs/components)
- [Ionic Forum](https://forum.ionicframework.com/)
- [Angular Documentation](https://angular.io/docs)

---
End of Lab
