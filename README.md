# Ionic UI Components Lab: Essential Components in Action

## Overview

In this lab, you'll learn about 7 essential Ionic UI components by building elements of a music discovery app. Each section follows a pattern of demonstration followed by hands-on exercises to deepen your understanding.

## Contents
1. [Project Setup](#project-setup)
2. [Ion-Tabs: App Navigation](#ion-tabs-app-navigation)
3. [Ion-Card: Rich Content Display](#ion-card-rich-content-display)
4. [Ion-List: Interactive Lists](#ion-list-interactive-lists)
5. [Ion-Modal: Detailed Views](#ion-modal-detailed-views)
6. [Ion-Accordion: Collapsible Content](#ion-accordion-collapsible-content)
7. [Ion-Infinite-Scroll: Dynamic Loading](#ion-infinite-scroll-dynamic-loading)
8. [Ion-Refresher: Content Updates](#ion-refresher-content-updates)
9. [Testing Your Components](#testing-your-components)

## Project Setup

Create new project with tabs:
```bash
ionic start ui-components-demo tabs --type=angular
cd ui-components-demo
```

## Ion-Tabs: App Navigation

### Introduction
Ion-Tabs provide a consistent way to organize and navigate between different sections of your app. We'll create a basic tab structure and then enhance it with additional features.

### Demo Implementation

Create file: `src/app/tabs/tabs.page.ts`
```typescript
import { Component } from '@angular/core';
import { IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel } from '@ionic/angular/standalone';
import { Disc, Library, Heart } from 'lucide-react';

@Component({
  selector: 'app-tabs',
  templateUrl: './tabs.page.html',
  styleUrls: ['./tabs.page.scss'],
  standalone: true,
  imports: [IonTabs, IonTabBar, IonTabButton, IonIcon, IonLabel]
})
export class TabsPage {
  favoriteCount = 5;
  
  showTabOptions(event: any) {
    console.log('Long press detected', event);
    // Implement your long-press logic here
  }
}
```

Create file: `src/app/tabs/tabs.page.html`
```html
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
```

Create file: `src/app/tabs/tabs.page.scss`
```scss
:host {
  --ion-tab-bar-color-selected: #ff4081;
}

ion-tab-button {
  transition: all 0.3s ease;
  
  &.tab-selected {
    transform: scale(1.05);
  }
}
```

### Key Features
- Bottom tab navigation
- Icon and label support
- Active tab indication
- Route integration

### DIY Exercise: Enhance the Tab Bar

1. Add a badge counter to the Favorites tab

Update `tabs.page.html`:
```html
<ion-tab-button tab="favorites">
  <ion-icon><Heart /></ion-icon>
  <ion-label>Favorites</ion-label>
  <ion-badge>{{ favoriteCount }}</ion-badge>
</ion-tab-button>
```

2. Create a custom tab indicator

Update `tabs.page.scss`:
```scss
:host {
  --ion-tab-bar-color-selected: #ff4081;
}

ion-tab-button {
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    width: 0;
    height: 3px;
    background: var(--ion-tab-bar-color-selected);
    transition: all 0.3s ease;
  }

  &.tab-selected::before {
    width: 50%;
    left: 25%;
  }
}
```

3. Add a long-press action to each tab

Update `tabs.page.html`:
```html
<ion-tab-button 
  tab="discover" 
  (press)="showTabOptions($event)">
```

4. Implement tab switching animations

Update `tabs.page.ts`:
```typescript
import { trigger, transition, style, animate } from '@angular/animations';

@Component({
  animations: [
    trigger('tabSwitch', [
      transition(':enter', [
        style({ transform: 'translateY(20px)', opacity: 0 }),
        animate('200ms ease-out', style({ transform: 'translateY(0)', opacity: 1 }))
      ])
    ])
  ]
})
```

## Ion-Card: Rich Content Display

### Introduction
Ion-Card is perfect for displaying rich content with images and actions. We'll create an album card component and then extend it with additional features.

### Demo Implementation

Create file: `src/app/components/album-card/album-card.component.ts`:
```typescript
import { Component, Input } from '@angular/core';
import { 
  IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, 
  IonCardContent, IonButton, IonIcon, IonBadge, IonProgressBar 
} from '@ionic/angular/standalone';

interface Album {
  title: string;
  artist: string;
  coverUrl: string;
  year: number;
  playCount?: number;
  progress?: number;
}

@Component({
  selector: 'app-album-card',
  templateUrl: './album-card.component.html',
  styleUrls: ['./album-card.component.scss'],
  standalone: true,
  imports: [
    IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle,
    IonCardContent, IonButton, IonIcon, IonBadge, IonProgressBar
  ]
})
export class AlbumCardComponent {
  @Input() album!: Album;
  @Input() featured = false;
}
```

Create file: `src/app/components/album-card/album-card.component.html`:
```html
<ion-card [class.featured]="featured">
  <img [alt]="album.title" [src]="album.coverUrl"/>
  
  <ion-badge *ngIf="album.playCount" class="absolute top-2 right-2">
    {{ album.playCount }} plays
  </ion-badge>
  
  <ion-card-header>
    <ion-card-title>{{ album.title }}</ion-card-title>
    <ion-card-subtitle>{{ album.artist }}</ion-card-subtitle>
  </ion-card-header>
  
  <ion-card-content>
    <div class="flex justify-between items-center mb-4">
      <span>{{ album.year }}</span>
      <ion-button fill="clear">
        <ion-icon slot="icon-only" name="heart"></ion-icon>
      </ion-button>
    </div>
    
    <ion-progress-bar 
      *ngIf="album.progress"
      [value]="album.progress" 
      color="primary">
    </ion-progress-bar>
    
    <div class="actions mt-4">
      <ion-button fill="clear" size="small">
        <ion-icon slot="start" name="share-outline"></ion-icon>
        Share
      </ion-button>
      <ion-button fill="clear" size="small">
        <ion-icon slot="start" name="add-outline"></ion-icon>
        Add to Playlist
      </ion-button>
    </div>
  </ion-card-content>
</ion-card>
```

Create file: `src/app/components/album-card/album-card.component.scss`:
```scss
ion-card {
  margin: 16px;
  border-radius: 12px;
  overflow: hidden;
  transition: transform 0.2s ease;

  &:hover {
    transform: translateY(-2px);
  }

  &.featured {
    margin: 24px;
    
    img {
      height: 300px;
      object-fit: cover;
    }
    
    ion-card-content {
      padding: 16px;
    }
  }
}

.actions {
  display: flex;
  justify-content: space-around;
  border-top: 1px solid var(--ion-color-light);
  padding-top: 8px;
}
```

## Ion-List: Interactive Lists

### Introduction
Ion-List provides a consistent way to display rows of information with interactive features. We'll create a song list with sliding actions.

### Demo Implementation

Create file: `src/app/components/song-list/song-list.component.ts`:
```typescript
import { Component, Input } from '@angular/core';
import { 
  IonList, IonItem, IonLabel, IonNote, IonItemSliding,
  IonItemOptions, IonItemOption, IonIcon, IonReorderGroup,
  IonReorder, IonItemDivider
} from '@ionic/angular/standalone';

interface Song {
  title: string;
  artist: string;
  duration: string;
  category?: string;
}

@Component({
  selector: 'app-song-list',
  templateUrl: './song-list.component.html',
  styleUrls: ['./song-list.component.scss'],
  standalone: true,
  imports: [
    IonList, IonItem, IonLabel, IonNote, IonItemSliding,
    IonItemOptions, IonItemOption, IonIcon, IonReorderGroup,
    IonReorder, IonItemDivider
  ]
})
export class SongListComponent {
  @Input() songs: Song[] = [];
  reorderEnabled = false;

  handleReorder(event: any) {
    // Prevent default behavior and handle the reorder
    event.detail.complete(true);
  }

  playSong(song: Song) {
    console.log('Playing:', song.title);
  }

  addToPlaylist(song: Song) {
    console.log('Adding to playlist:', song.title);
  }
}
```

Create file: `src/app/components/song-list/song-list.component.html`:
```html
<ion-list>
  <ion-reorder-group [disabled]="!reorderEnabled" (ionItemReorder)="handleReorder($event)">
    <ng-container *ngFor="let category of getSongCategories()">
      <ion-item-divider sticky>
        <ion-label>{{ category }}</ion-label>
      </ion-item-divider>

      <ion-item-sliding *ngFor="let song of getSongsByCategory(category)">
        <ion-item>
          <ion-label>
            <h2>{{ song.title }}</h2>
            <p>{{ song.artist }}</p>
          </ion-label>
          <ion-note slot="end">{{ song.duration }}</ion-note>
          <ion-reorder slot="end"></ion-reorder>
        </ion-item>

        <ion-item-options side="start">
          <ion-item-option color="success" (click)="playSong(song)">
            <ion-icon slot="icon-only" name="play"></ion-icon>
          </ion-item-option>
        </ion-item-options>

        <ion-item-options side="end">
          <ion-item-option color="primary" (click)="addToPlaylist(song)">
            <ion-icon slot="icon-only" name="add"></ion-icon>
          </ion-item-option>
        </ion-item-options>
      </ion-item-sliding>
    </ng-container>
  </ion-reorder-group>
</ion-list>
```

Create file: `src/app/components/song-list/song-list.component.scss`:
```scss
ion-item-divider {
  --background: var(--ion-color-light);
  --color: var(--ion-color-dark);
  font-weight: 600;
  letter-spacing: 0.5px;
}

ion-item {
  --padding-start: 16px;
  --inner-padding-end: 16px;
  
  ion-label {
    h2 {
      font-weight: 500;
      margin-bottom: 4px;
    }
    
    p {
      color: var(--ion-color-medium);
    }
  }
}

ion-note {
  font-size: 0.85em;
  color: var(--ion-color-medium);
}
```

## Ion-Modal: Detailed Views

### Introduction
Ion-Modal provides a way to display detailed content without leaving the current context. We'll create a song details modal.

### Demo Implementation

Create file: `src/app/components/song-details-modal/song-details-modal.component.ts`:
```typescript
import { Component, Input } from '@angular/core';
import {
  IonModal, IonHeader, IonToolbar, IonTitle, IonButtons,
  IonButton, IonContent, IonIcon
} from '@ionic/angular/standalone';
import { trigger, transition, style, animate } from '@angular/animations';

@Component({
  selector: 'app-song-details-modal',
  templateUrl: './song-details-modal.component.html',
  styleUrls: ['./song-details-modal.component.scss'],
  standalone: true,
  imports: [
    IonModal, IonHeader, IonToolbar, IonTitle, IonButtons,
    IonButton, IonContent, IonIcon
  ],
  animations: [
    trigger('modalAnimation', [
      transition(':enter', [
        style({ transform: 'translateY(100%)' }),
        animate('300ms ease-out', style({ transform: 'translateY(0)' }))
      ]),
      transition(':leave', [
        animate('200ms ease-in', style({ transform: 'translateY(100%)' }))
      ])
    ])
  ]
})
export class SongDetailsModalComponent {
  @Input() isOpen = false;
  presentingElement: HTMLElement | null = null;

  constructor() {
    this.presentingElement = document.querySelector('.ion-page');
  }

  onWillPresent() {
    console.log('Modal will present');
  }

  onDidDismiss() {
    console.log('Modal dismissed');
    this.isOpen = false;
  }
}
```

Create file: `src/app/components/song-details-modal/song-details-modal.component.html`:
```html
<ion-modal 
  [isOpen]="isOpen"
  [breakpoints]="[0, 0.25, 0.5, 0.75]"
  [initialBreakpoint]="0.5"
  [canDismiss]="true"
  [presentingElement]="presentingElement"
  (ionModalWillPresent)="onWillPresent()"
  (ionModalDidDismiss)="onDidDismiss()"
  [@modalAnimation]>
  
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
      <div class="song-details">
        <img src="album-cover.jpg" alt="Album Cover" class="album-cover"/>
        <h2>Song Title</h2>
        <p class="artist">Artist Name</p>
        
        <div class="metadata">
          <div class="info-item">
            <ion-icon name="time-outline"></ion-icon>
            <span>3:45</span>
          </div>
          <div class="info-item">
            <ion-icon name="musical-notes-outline"></ion-icon>
            <span>Album Name</span>
          </div>
          <div class="info-item">
            <ion-icon name="calendar-outline"></ion-icon>
            <span>2024</span>
          </div>
        </div>
        
        <div class="actions">
          <ion-button expand="block" color="primary">
            <ion-icon slot="start" name="play"></ion-icon>
            Play Now
          </ion-button>
          <ion-button expand="block" fill="outline">
            <ion-icon slot="start" name="add"></ion-icon>
            Add to Playlist
          </ion-button>
        </div>
      </div>
    </ion-content>
  </ng-template>
</ion-modal>
```

Create file: `src/app/components/song-details-modal/song-details-modal.component.scss`:
```scss
.song-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 16px;

  .album-cover {
    width: 200px;
    height: 200px;
    border-radius: 8px;
    margin-bottom: 16px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  h2 {
    font-size: 24px;
    margin: 8px 0;
  }

  .artist {
    color: var(--ion-color-medium);
    margin-bottom: 16px;
  }

  .metadata {
    width: 100%;
    display: flex;
    justify-content: space-around;
    margin: 24px 0;

    .info-item {
      display: flex;
      align-items: center;
      gap: 8px;
      color: var(--ion-color-medium);

      ion-icon {
        font-size: 20px;
      }
    }
  }

  .actions {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-top: 24px;
  }
}
```

## Ion-Accordion: Collapsible Content

### Introduction
Ion-Accordion helps organize content into expandable sections. We'll create a genre-based music browser.

### Demo Implementation

Create file: `src/app/components/genre-browser/genre-browser.component.ts`:
```typescript
import { Component } from '@angular/core';
import {
  IonAccordionGroup, IonAccordion, IonItem, IonLabel,
  IonBadge, IonList, IonIcon
} from '@ionic/angular/standalone';

interface Genre {
  name: string;
  count: number;
  subgenres?: Genre[];
  albums: Album[];
}

interface Album {
  title: string;
  artist: string;
}

@Component({
  selector: 'app-genre-browser',
  templateUrl: './genre-browser.component.html',
  styleUrls: ['./genre-browser.component.scss'],
  standalone: true,
  imports: [
    IonAccordionGroup, IonAccordion, IonItem, IonLabel,
    IonBadge, IonList, IonIcon
  ]
})
export class GenreBrowserComponent {
  genres: Genre[] = [
    {
      name: 'Rock',
      count: 12,
      subgenres: [
        { name: 'Alternative', count: 5, albums: [] },
        { name: 'Classic Rock', count: 7, albums: [] }
      ],
      albums: []
    }
  ];

  isOpen = false;
}
```

Create file: `src/app/components/genre-browser/genre-browser.component.html`:
```html
<ion-accordion-group>
  <ion-accordion *ngFor="let genre of genres">
    <ion-item slot="header" [class.expanded]="isOpen">
      <ion-label>{{ genre.name }}</ion-label>
      <ion-badge slot="end">{{ genre.count }}</ion-badge>
      <ion-icon 
        slot="end" 
        [name]="isOpen ? 'chevron-down' : 'chevron-forward'">
      </ion-icon>
    </ion-item>
    
    <div class="ion-padding" slot="content">
      <ion-accordion-group *ngIf="genre.subgenres?.length">
        <ion-accordion *ngFor="let subgenre of genre.subgenres">
          <ion-item slot="header">
            <ion-label>{{ subgenre.name }}</ion-label>
            <ion-badge slot="end">{{ subgenre.count }}</ion-badge>
          </ion-item>
          
          <ion-list slot="content">
            <ion-item *ngFor="let album of subgenre.albums">
              <ion-label>
                <h3>{{ album.title }}</h3>
                <p>{{ album.artist }}</p>
              </ion-label>
            </ion-item>
          </ion-list>
        </ion-accordion>
      </ion-accordion-group>
      
      <ion-list *ngIf="genre.albums?.length">
        <ion-item *ngFor="let album of genre.albums">
          <ion-label>
            <h3>{{ album.title }}</h3>
            <p>{{ album.artist }}</p>
          </ion-label>
        </ion-item>
      </ion-list>
    </div>
  </ion-accordion>
</ion-accordion-group>
```

Create file: `src/app/components/genre-browser/genre-browser.component.scss`:
```scss
ion-accordion {
  margin: 8px 0;
}

ion-item.expanded {
  --background: var(--ion-color-light);
}

ion-icon[slot="end"] {
  transition: transform 0.3s ease;
}

.expanded ion-icon[slot="end"] {
  transform: rotate(180deg);
}

ion-badge {
  margin-right: 8px;
}

ion-list ion-item {
  --padding-start: 32px;
}
```

## Ion-Infinite-Scroll: Dynamic Loading

### Introduction
Ion-Infinite-Scroll enables continuous content loading. We'll create an infinite scrolling album list.

### Demo Implementation

Create file: `src/app/components/infinite-album-list/infinite-album-list.component.ts`:
```typescript
import { Component, OnInit } from '@angular/core';
import {
  IonContent, IonList, IonItem, IonLabel,
  IonInfiniteScroll, IonInfiniteScrollContent
} from '@ionic/angular/standalone';

@Component({
  selector: 'app-infinite-album-list',
  templateUrl: './infinite-album-list.component.html',
  styleUrls: ['./infinite-album-list.component.scss'],
  standalone: true,
  imports: [
    IonContent, IonList, IonItem, IonLabel,
    IonInfiniteScroll, IonInfiniteScrollContent
  ]
})
export class InfiniteAlbumListComponent implements OnInit {
  items: any[] = [];
  loading = false;
  error: string | null = null;

  ngOnInit() {
    this.loadInitialData();
  }

  async loadInitialData() {
    try {
      await this.loadData();
    } catch (error) {
      this.error = 'Failed to load initial data';
    }
  }

  async loadData() {
    this.loading = true;
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 1000));
      const newItems = Array(10).fill(null).map((_, i) => ({
        id: this.items.length + i,
        title: `Album ${this.items.length + i + 1}`
      }));
      this.items.push(...newItems);
    } finally {
      this.loading = false;
    }
  }

  async loadMore(event: any) {
    try {
      await this.loadData();
    } catch (error) {
      this.error = 'Failed to load more data';
    } finally {
      event.target.complete();
    }
  }
}
```

Create file: `src/app/components/infinite-album-list/infinite-album-list.component.html`:
```html
<ion-content>
  <ion-list>
    <ion-item *ngFor="let item of items">
      <ion-label>{{ item.title }}</ion-label>
    </ion-item>
  </ion-list>

  <div *ngIf="error" class="error-message">
    {{ error }}
    <ion-button (click)="loadInitialData()">Retry</ion-button>
  </div>

  <ion-infinite-scroll
    threshold="100px"
    position="bottom"
    (ionInfinite)="loadMore($event)">
    <ion-infinite-scroll-content
      loadingSpinner="bubbles"
      loadingText="Loading more albums...">
    </ion-infinite-scroll-content>
  </ion-infinite-scroll>
</ion-content>
```

Create file: `src/app/components/infinite-album-list/infinite-album-list.component.scss`:
```scss
.error-message {
  text-align: center;
  padding: 16px;
  color: var(--ion-color-danger);
}

ion-infinite-scroll-content {
  min-height: 80px;
}
```

## Ion-Refresher: Content Updates

### Introduction
Ion-Refresher provides pull-to-refresh functionality. We'll create a refreshable music library.

### Demo Implementation

Create file: `src/app/components/refreshable-library/refreshable-library.component.ts`:
```typescript
import { Component } from '@angular/core';
import {
  IonContent, IonRefresher, IonRefresherContent,
  IonList, IonItem, IonLabel
} from '@ionic/angular/standalone';

@Component({
  selector: 'app-refreshable-library',
  templateUrl: './refreshable-library.component.html',
  styleUrls: ['./refreshable-library.component.scss'],
  standalone: true,
  imports: [
    IonContent, IonRefresher, IonRefresherContent,
    IonList, IonItem, IonLabel
  ]
})
export class RefreshableLibraryComponent {
  items: any[] = [];
  loading = false;

  constructor() {
    this.loadInitialData();
  }

  async loadInitialData() {
    // Simulate initial data load
    this.items = Array(20).fill(null).map((_, i) => ({
      id: i,
      title: `Track ${i + 1}`
    }));
  }

  async doRefresh(event: any) {
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 1500));
      
      // Update data
      this.items.unshift({
        id: this.items.length,
        title: `New Track ${this.items.length + 1}`
      });
      
      this.showUpdateToast();
    } catch (error) {
      this.showErrorAlert();
    } finally {
      event.target.complete();
    }
  }

  private showUpdateToast() {
    // Implement toast notification
    console.log('Library updated');
  }

  private showErrorAlert() {
    // Implement error alert
    console.error('Failed to refresh library');
  }
}
```

Create file: `src/app/components/refreshable-library/refreshable-library.component.html`:
```html
<ion-content>
  <ion-refresher slot="fixed" (ionRefresh)="doRefresh($event)">
    <ion-refresher-content
      pullingIcon="chevron-down"
      pullingText="Pull to refresh"
      refreshingSpinner="circles"
      refreshingText="Updating library...">
    </ion-refresher-content>
  </ion-refresher>

  <ion-list>
    <ion-item *ngFor="let item of items">
      <ion-label>{{ item.title }}</ion-label>
    </ion-item>
  </ion-list>
</ion-content>
```

Create file: `src/app/components/refreshable-library/refreshable-library.component.scss`:
```scss
ion-refresher {
  z-index: 1;
}

ion-list {
  padding-top: 1px; // Prevent margin collapse
}

ion-item {
  --padding-start: 16px;
  --inner-padding-end: 16px;
  
  &:last-child {
    --border-width: 0;
  }
}
```

## Testing Your Components

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

## Final Challenge: Build a Complete Feature

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
