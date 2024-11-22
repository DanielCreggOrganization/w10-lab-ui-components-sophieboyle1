# Ionic UI Components Lab: Essential Components in Action

## Overview

In this lab, you'll learn about 7 essential Ionic UI components by building elements of a music discovery app. Each section follows a pattern of demonstration followed by hands-on exercises to deepen your understanding.

## Contents
1. [Project Setup](#1-project-setup)
2. [Ion-Tabs: App Navigation](#2-ion-tabs)
3. [Ion-Card: Rich Content Display](#3-ion-card)
4. [Ion-List: Interactive Lists](#4-ion-list)
5. [Ion-Modal: Detailed Views](#5-ion-modal)
6. [Ion-Accordion: Collapsible Content](#6-ion-accordion)
7. [Ion-Infinite-Scroll: Dynamic Loading](#7-ion-infinite-scroll)
8. [Ion-Refresher: Content Updates](#8-ion-refresher)
9. [Testing Your Components](#9-testing-your-components)

## 1. Project Setup

Create new project with tabs:
```bash
ionic start ui-components-demo tabs --type=angular
cd ui-components-demo
```

## 2. Ion-Tabs: App Navigation

### Introduction
Ion-Tabs provide a consistent way to organize and navigate between different sections of your app. We'll create a basic tab structure and then enhance it with additional features.

### Demo Implementation
```typescript
// src/app/tabs/tabs.page.ts
import { Component } from '@angular/core';
import { IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel } from '@ionic/angular/standalone';
import { Disc, Library, Heart } from 'lucide-react';

@Component({
  template: `
    <ion-tabs>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="discover">
          <ion-icon><Disc /></ion-icon>
          <ion-label>Discover</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="library">
          <ion-icon><Library /></ion-icon>
          <ion-label>Library</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="favorites">
          <ion-icon><Heart /></ion-icon>
          <ion-label>Favorites</ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  `,
  standalone: true,
  imports: [IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel]
})
export class TabsPage {}
```

### Key Features
- Bottom tab navigation
- Icon and label support
- Active tab indication
- Route integration

### DIY Exercise: Enhance the Tab Bar

Now that you understand basic tab implementation, enhance it with these features:

1. Add a badge counter to the Favorites tab
```typescript
// Hint: Use IonBadge to show count
<ion-tab-button tab="favorites">
  <ion-icon><Heart /></ion-icon>
  <ion-label>Favorites</ion-label>
  <ion-badge>{{ favoriteCount }}</ion-badge>
</ion-tab-button>
```

2. Create a custom tab indicator
```typescript
// Hint: Use CSS custom properties
:host {
  --ion-tab-bar-color-selected: #ff4081;
}
```

3. Add a long-press action to each tab
```typescript
// Hint: Use (press) event
<ion-tab-button 
  tab="discover" 
  (press)="showTabOptions($event)">
```

4. Implement tab switching animations
```typescript
// Hint: Use Angular animations
@Component({
  animations: [
    trigger('tabSwitch', [
      transition(':enter', [ ... ])
    ])
  ]
})
```

### Bonus Challenge
Create a custom tab bar that changes appearance based on the current route:
- Different background colors per tab
- Custom icons for active state
- Micro-interactions on tab change

## 3. Ion-Card: Rich Content Display

### Introduction
Ion-Card is perfect for displaying rich content with images and actions. We'll create an album card component and then extend it with additional features.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-card>
      <img [alt]="album.title" [src]="album.coverUrl"/>
      <ion-card-header>
        <ion-card-title>{{ album.title }}</ion-card-title>
        <ion-card-subtitle>{{ album.artist }}</ion-card-subtitle>
      </ion-card-header>
      <ion-card-content>
        <div class="flex justify-between">
          <span>{{ album.year }}</span>
          <ion-button fill="clear">
            <ion-icon slot="icon-only" name="heart"></ion-icon>
          </ion-button>
        </div>
      </ion-card-content>
    </ion-card>
  `,
  imports: [
    IonCard,
    IonCardHeader,
    IonCardTitle,
    IonCardSubtitle,
    IonCardContent,
    IonButton,
    IonIcon
  ]
})
```

### Key Features
- Header with title/subtitle
- Media content support
- Action buttons
- Flexible content area
- Shadow and elevation

### DIY Exercise: Enhance the Album Card

1. Add a "Play Count" badge
```typescript
// Hint: Use IonBadge in the top-right corner
<ion-badge class="absolute top-2 right-2">
  {{ playCount }} plays
</ion-badge>
```

2. Add a progress bar
```typescript
// Hint: Use IonProgressBar
<ion-progress-bar 
  [value]="albumProgress" 
  color="primary">
</ion-progress-bar>
```

3. Create a card footer
```typescript
// Hint: Add action buttons
<ion-buttons slot="end">
  <ion-button>
    <ion-icon name="share-outline"></ion-icon>
  </ion-button>
</ion-buttons>
```

### Bonus Challenge
Create a Featured Album card variant with:
- Larger image size
- Album description
- Rating display
- Top tracks preview

## 4. Ion-List: Interactive Lists

### Introduction
Ion-List provides a consistent way to display rows of information with interactive features. We'll create a song list with sliding actions.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-list>
      <ion-item-sliding>
        <ion-item>
          <ion-label>
            <h2>Song Title</h2>
            <p>Artist Name</p>
          </ion-label>
          <ion-note slot="end">3:45</ion-note>
        </ion-item>

        <ion-item-options side="end">
          <ion-item-option color="primary">
            <ion-icon slot="icon-only" name="add"></ion-icon>
          </ion-item-option>
        </ion-item-options>
      </ion-item-sliding>
    </ion-list>
  `,
  imports: [
    IonList,
    IonItem,
    IonLabel,
    IonNote,
    IonItemSliding,
    IonItemOptions,
    IonItemOption,
    IonIcon
  ]
})
```

### Key Features
- Sliding actions
- Custom item layouts
- Interactive feedback
- Reorder capability

### DIY Exercise: Enhance the Song List

1. Add reorder functionality
```typescript
// Hint: Use IonReorderGroup
<ion-reorder-group 
  (ionItemReorder)="handleReorder($event)" 
  disabled="false">
```

2. Add multiple sliding actions
```typescript
// Hint: Add options to both sides
<ion-item-options side="start">
  <ion-item-option color="success">
    <ion-icon name="play"></ion-icon>
  </ion-item-option>
</ion-item-options>
```

3. Add item dividers with sticky headers
```typescript
// Hint: Use IonItemDivider
<ion-item-divider sticky>
  <ion-label>Recently Added</ion-label>
</ion-item-divider>
```

### Bonus Challenge
Create a playlist view that combines:
- Reorderable tracks
- Multiple action options
- Progress indicators
- Playing status

## 5. Ion-Modal: Detailed Views

### Introduction
Ion-Modal provides a way to display detailed content without leaving the current context. We'll create a song details modal.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-button (click)="isOpen = true">
      View Details
    </ion-button>

    <ion-modal 
      [isOpen]="isOpen"
      [breakpoints]="[0, 0.25, 0.5, 0.75]"
      [initialBreakpoint]="0.5"
    >
      <ng-template>
        <ion-header>
          <ion-toolbar>
            <ion-title>Song Details</ion-title>
            <ion-buttons slot="end">
              <ion-button (click)="isOpen = false">Close</ion-button>
            </ion-buttons>
          </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
          <h2>Song Title</h2>
          <p>Song details...</p>
        </ion-content>
      </ng-template>
    </ion-modal>
  `,
  imports: [
    IonModal,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonButtons,
    IonButton,
    IonContent
  ]
})
```

### Key Features
- Sheet modal with breakpoints
- Customizable transitions
- Header/footer areas
- Backdrop dismissal

### DIY Exercise: Enhance the Modal

1. Add gesture-based dismissal
```typescript
// Hint: Use swipe gesture
<ion-modal 
  [canDismiss]="true"
  [presentingElement]="presentingElement">
```

2. Add custom transitions
```typescript
// Hint: Use enterAnimation/leaveAnimation
[enterAnimation]="customEnterAnimation"
[leaveAnimation]="customLeaveAnimation"
```

3. Add modal lifecycle hooks
```typescript
// Hint: Use modal events
(ionModalWillPresent)="onWillPresent()"
(ionModalDidDismiss)="onDidDismiss()"
```

### Bonus Challenge
Create an album details modal with:
- Image gallery
- Track list
- Artist information
- Related albums

## 6. Ion-Accordion: Collapsible Content

### Introduction
Ion-Accordion helps organize content into expandable sections. We'll create a genre-based music browser.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-accordion-group>
      <ion-accordion value="first">
        <ion-item slot="header">
          <ion-label>Rock</ion-label>
          <ion-badge slot="end">12</ion-badge>
        </ion-item>
        
        <div class="ion-padding" slot="content">
          <ion-list>
            <ion-item>Album 1</ion-item>
            <ion-item>Album 2</ion-item>
          </ion-list>
        </div>
      </ion-accordion>
    </ion-accordion-group>
  `,
  imports: [
    IonAccordionGroup,
    IonAccordion,
    IonItem,
    IonLabel,
    IonBadge,
    IonList
  ]
})
```

### Key Features
- Expandable sections
- Custom headers
- Animation support
- Multiple/single open modes

### DIY Exercise: Enhance the Accordion

1. Add nested accordions
```typescript
// Hint: Create sub-categories
<ion-accordion-group>
  <ion-accordion>
    <ion-accordion-group slot="content">
      <ion-accordion>
        <!-- Sub-category content -->
      </ion-accordion>
    </ion-accordion-group>
  </ion-accordion>
</ion-accordion-group>
```

2. Add custom toggle icons
```typescript
// Hint: Use toggleIcon slot
<ion-icon 
  slot="toggleIcon" 
  [name]="isOpen ? 'chevron-down' : 'chevron-forward'">
</ion-icon>
```

3. Add transition animations
```typescript
// Hint: Use CSS transitions
.custom-accordion {
  transition: all 0.3s ease-in-out;
}
```

### Bonus Challenge
Create a music browser with:
- Nested genre categories
- Preview playback
- Drag-and-drop organization
- Custom animations

## 7. Ion-Infinite-Scroll: Dynamic Loading

### Introduction
Ion-Infinite-Scroll enables continuous content loading. We'll create an infinite scrolling album list.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-content>
      <ion-list>
        @for (item of items; track item) {
          <ion-item>{{ item }}</ion-item>
        }
      </ion-list>

      <ion-infinite-scroll (ionInfinite)="loadMore($event)">
        <ion-infinite-scroll-content
          loadingSpinner="bubbles"
          loadingText="Loading more...">
        </ion-infinite-scroll-content>
      </ion-infinite-scroll>
    </ion-content>
  `,
  imports: [
    IonContent,
    IonList,
    IonItem,
    IonInfiniteScroll,
    IonInfiniteScrollContent
  ]
})
```

### Key Features
- Automatic threshold detection
- Custom loading indicators
- Completion handling
- Disable on completion

### DIY Exercise: Enhance the Infinite Scroll

1. Add custom threshold
```typescript
// Hint: Set threshold distance
<ion-infinite-scroll threshold="100px">
```

2. Add custom loading spinner
```typescript
// Hint: Customize loading state
<ion-infinite-scroll-content
  [spinnerIcon]="customSpinner">
```

3. Add error handling
```typescript
// Hint: Handle loading errors
async loadMore(event) {
  try {
    await this.loadData();
  } catch (e) {
    this.showError();
  } finally {
    event.target.complete();
  }
}
```

### Bonus Challenge
Create an advanced infinite scroll that:
- Maintains scroll position
- Prefetches content
- Shows loading skeletons
- Handles errors gracefully

## 8. Ion-Refresher: Content Updates

### Introduction
Ion-Refresher provides pull-to-refresh functionality. We'll create a refreshable music library.

### Demo Implementation
```typescript
@Component({
  template: `
    <ion-content>
      <ion-refresher slot="fixed" (ionRefresh)="doRefresh($event)">
        <ion-refresher-content
          pullingIcon="chevron-down"
          pullingText="Pull to refresh"
          refreshingSpinner="circles"
          refreshingText="Refreshing...">
        </ion-refresher-content>
      </ion-refresher>

      <ion-list>
        @for (item of items; track item) {
          <ion-item>{{ item }}</ion-item>
        }
      </ion-list>
    </ion-content>
  `,
  imports: [
    IonContent,
    IonRefresher,
    IonRefresherContent,
    IonList,
    IonItem
  ]
})
```

### Key Features
- Pull gesture detection
- Custom pulling states
- Loading indicators
- Completion handling

### DIY Exercise: Enhance the Refresher

1. Add custom pull indicator
```typescript
// Hint: Create custom pulling icon
<ion-refresher-content
  [pullingIcon]="customIcon">
```

2. Add progress indication
```typescript
// Hint: Show loading progress
(ionPull)="updateProgress($event)"
```

3. Add completion animation
```typescript
// Hint: Add success animation
<ion-refresher-content
  [pullingIcon]="customIcon"
  [refreshingSpinner]="customSpinner"
  (ionRefreshComplete)="showSuccessAnimation()">
</ion-refresher-content>
```

4. Add custom refresh logic
```typescript
// Hint: Implement refresh strategy
async doRefresh(event: any) {
  try {
    await this.fetchLatestData();
    this.showUpdateToast();
  } catch (error) {
    this.showErrorAlert();
  } finally {
    event.target.complete();
  }
}
```

### Bonus Challenge
Create an advanced refresh implementation that:
- Shows what's being updated
- Includes a progress indicator
- Animates new content arrival
- Provides visual feedback on success/failure

## 9. Testing Your Components

### Browser Testing
1. Run your app:
   ```bash
   ionic serve
   ```

2. Test each component:
   - Verify all interactions work
   - Check responsive behavior
   - Test error states
   - Validate animations

### Common Issues and Solutions
1. Modal not displaying properly
   ```typescript
   // Ensure presenting element is set
   constructor() {
     this.presentingElement = document.querySelector('.ion-page');
   }
   ```

2. Infinite scroll not triggering
   ```typescript
   // Add proper threshold
   <ion-infinite-scroll threshold="15%">
   ```

3. Accordion animation glitches
   ```typescript
   // Add will-change property
   .ion-accordion-content {
     will-change: height;
   }
   ```

## 10. Final Challenge: Build a Complete Feature

Combine all the components you've learned about to create a "Featured Albums" section that includes:

1. A tab for featured content
2. Cards displaying album artwork
3. A list of top tracks
4. Modal for album details
5. Accordion for album information
6. Infinite scroll for more albums
7. Pull-to-refresh for updates

### Requirements:
- Use at least 5 of the UI components covered
- Implement custom styling
- Include animations
- Handle errors gracefully
- Follow Ionic best practices

### Example Structure:
```typescript
@Component({
  template: `
    <ion-content>
      <ion-refresher>...</ion-refresher>
      
      <ion-accordion-group>
        <ion-accordion>
          <ion-item slot="header">
            <ion-label>Featured Albums</ion-label>
          </ion-item>
          
          <div slot="content">
            <ion-card>...</ion-card>
          </div>
        </ion-accordion>
      </ion-accordion-group>

      <ion-list>...</ion-list>
      
      <ion-infinite-scroll>...</ion-infinite-scroll>
      
      <ion-modal>...</ion-modal>
    </ion-content>
  `
})
```

## Additional Resources

1. Official Documentation
   - [Ionic UI Components](https://ionicframework.com/docs/components)
   - [Angular Standalone Components](https://angular.io/guide/standalone-components)

2. Helpful Tools
   - [Ionic DevApp](https://ionicframework.com/docs/developing/previewing)
   - [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools)

3. Community Resources
   - [Ionic Forum](https://forum.ionicframework.com/)
   - [Stack Overflow - Ionic](https://stackoverflow.com/questions/tagged/ionic-framework)

---
End of Lab
