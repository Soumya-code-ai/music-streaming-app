# 🔧 MusicStream - Code Snippets & Common Tasks

## Quick Reference Guide

### 1. Playing a Song

**Component:**
```typescript
playSong(song: ISong): void {
  this.musicService.playSong(song);
}
```

**Service:**
```typescript
playSong(song: ISong): void {
  song.incrementPlayCount();
  this.currentSongSubject.next(song);
  this.isPlayingSubject.next(true);
}
```

**Template:**
```html
<button (click)="playSong(song)">▶️ Play</button>
```

---

### 2. Toggling Favorite

**Component:**
```typescript
toggleFavorite(song: ISong): void {
  this.musicService.toggleFavoriteSong(song.id);
  // Trigger UI update
  this.songs.set([...this.songs()]);
}
```

**Service:**
```typescript
toggleFavoriteSong(songId: string): void {
  const song = this.getSongById(songId);
  if (song) {
    song.toggleFavorite();
    this.updateFavoriteCount();
  }
}
```

**Template:**
```html
<button 
  [class.favorited]="song.isFavorite"
  (click)="toggleFavorite(song)">
  {{ song.isFavorite ? '❤️' : '🤍' }}
</button>
```

---

### 3. Creating a Playlist

**Component:**
```typescript
createPlaylist(): void {
  if (this.newPlaylistName().trim()) {
    this.musicService.createPlaylist(
      this.newPlaylistName(),
      this.newPlaylistDescription(),
      this.newPlaylistType(),
      'https://via.placeholder.com/200?text=' + this.newPlaylistName()
    );
    this.toggleCreateForm();
  }
}
```

**Service:**
```typescript
createPlaylist(
  name: string,
  description: string,
  type: PlaylistType,
  imageUrl: string
): IPlaylist {
  const id = `playlist-${Date.now()}`;
  const playlist = new Playlist(id, name, description, type, imageUrl);
  const playlists = [...this.playlistsSubject.value, playlist];
  this.playlistsSubject.next(playlists);
  this.playlistCount.set(playlists.length);
  return playlist;
}
```

**Template:**
```html
<form (ngSubmit)="createPlaylist()">
  <input [(ngModel)]="newPlaylistName" placeholder="Name" />
  <textarea [(ngModel)]="newPlaylistDescription" placeholder="Description"></textarea>
  <select [(ngModel)]="newPlaylistType">
    <option *ngFor="let type of playlistTypeOptions" [value]="type">
      {{ type }}
    </option>
  </select>
  <button type="submit">Create</button>
</form>
```

---

### 4. Searching Songs

**Component:**
```typescript
get filteredSongs(): ISong[] {
  let filtered = this.songs();

  // Filter by search query
  if (this.searchQuery()) {
    const query = this.searchQuery().toLowerCase();
    filtered = filtered.filter(
      song =>
        song.title.toLowerCase().includes(query) ||
        song.artist.name.toLowerCase().includes(query) ||
        song.album.title.toLowerCase().includes(query)
    );
  }

  // Filter by genre
  if (this.selectedGenre()) {
    filtered = filtered.filter(song => song.genre === this.selectedGenre());
  }

  return filtered;
}

onSearchChange(query: string): void {
  this.searchQuery.set(query);
}

onGenreChange(genre: string): void {
  this.selectedGenre.set(genre);
}
```

**Template:**
```html
<input
  type="text"
  placeholder="Search songs, artists, albums..."
  [value]="searchQuery()"
  (input)="onSearchChange($event.target.value)" />

<select 
  [value]="selectedGenre()"
  (change)="onGenreChange($event.target.value)">
  <option value="">All Genres</option>
  <option *ngFor="let genre of genres" [value]="genre">
    {{ genre }}
  </option>
</select>

<div class="songs-grid">
  <div *ngFor="let song of filteredSongs">
    <!-- Song card content -->
  </div>
</div>
```

---

### 5. Managing Player Controls

**Component:**
```typescript
export class Player implements OnInit {
  currentSong = signal<ISong | null>(null);
  isPlaying = signal(false);
  currentTime = signal(0);
  volume = signal(70);
  
  progress = computed(() => {
    const song = this.currentSong();
    return song ? (this.currentTime() / song.duration) * 100 : 0;
  });

  play(): void {
    if (this.currentSong()) {
      this.musicService.resumeSong();
    }
  }

  pause(): void {
    this.musicService.pauseSong();
  }

  seek(time: number): void {
    this.musicService.setCurrentTime(time);
  }

  onProgressClick(event: any): void {
    if (this.currentSong()) {
      const progressBar = event.currentTarget;
      const clickX = event.offsetX;
      const width = progressBar.offsetWidth;
      const percentage = clickX / width;
      const newTime = percentage * this.currentSong()!.duration;
      this.seek(newTime);
    }
  }
}
```

**Template:**
```html
<div class="progress-bar" (click)="onProgressClick($event)">
  <div class="progress-fill" [style.width.%]="progress()"></div>
</div>

<button (click)="isPlaying() ? pause() : play()">
  {{ isPlaying() ? '⏸️' : '▶️' }}
</button>

<div class="volume-section">
  <input 
    type="range" 
    min="0" 
    max="100" 
    [value]="volume()"
    (change)="onVolumeChange($event)" />
  <span>{{ volume() }}%</span>
</div>
```

---

### 6. Subscribing to Observable State

**Component:**
```typescript
ngOnInit(): void {
  // Subscribe to songs
  this.musicService.getSongs().subscribe(songs => {
    this.songs.set(songs);
  });

  // Subscribe to current song
  this.musicService.currentSong$.subscribe(song => {
    this.currentSong.set(song);
  });

  // Subscribe to playing status
  this.musicService.isPlaying$.subscribe(isPlaying => {
    this.isPlaying.set(isPlaying);
  });

  // Subscribe to current time
  this.musicService.currentTime$.subscribe(time => {
    this.currentTime.set(time);
  });
}
```

---

### 7. Using Signals & Computed

**Component:**
```typescript
// Define signals
protected songs = signal<ISong[]>([]);
protected searchQuery = signal('');
protected selectedGenre = signal('');
protected showFavoritesOnly = signal(false);

// Computed derived state
protected favoriteSongs = computed(() => 
  this.songs().filter(s => s.isFavorite)
);

protected genreOptions = computed(() => 
  Array.from(new Set(this.songs().map(s => s.genre)))
);

// Update signals
updateSongs(newSongs: ISong[]): void {
  this.songs.set(newSongs);
}

// Use in template
get favCount() {
  return this.favoriteSongs().length;
}
```

**Template:**
```html
<!-- Display signal value -->
<h3>{{ songs().length }} Songs</h3>

<!-- Display computed value -->
<span>{{ favoriteSongs().length }} Favorites</span>

<!-- Use in binding -->
<div [class.empty]="songs().length === 0">
  <!-- Content -->
</div>

<!-- Loop through computed -->
<select>
  <option *ngFor="let genre of genreOptions()" [value]="genre">
    {{ genre }}
  </option>
</select>
```

---

### 8. Event Emitter for Parent Communication

**Child Component:**
```typescript
import { Component, output } from '@angular/core';

@Component({
  selector: 'app-header',
  // ...
})
export class Header {
  viewChange = output<'home' | 'artists' | 'playlists' | 'now-playing'>();

  navigateTo(view: 'home' | 'artists' | 'playlists' | 'now-playing'): void {
    this.viewChange.emit(view);
  }
}
```

**Parent Component:**
```typescript
@Component({
  selector: 'app-root',
  template: `<app-header (viewChange)="onViewChange($event)"></app-header>`,
})
export class App {
  currentView = signal<'home' | 'artists' | 'playlists' | 'now-playing'>('home');

  onViewChange(view: 'home' | 'artists' | 'playlists' | 'now-playing'): void {
    this.currentView.set(view);
  }
}
```

**Template:**
```html
<app-header (viewChange)="onViewChange($event)"></app-header>
```

---

### 9. Formatting Time Display

**Component:**
```typescript
formatDuration(seconds: number): string {
  const mins = Math.floor(seconds / 60);
  const secs = seconds % 60;
  return `${mins}:${secs.toString().padStart(2, '0')}`;
}

// Example: 200 seconds → "3:20"
```

**Template:**
```html
<span>⏱️ {{ formatDuration(song.duration) }}</span>
```

---

### 10. Conditional Class Binding

**Component:**
```typescript
isActive(view: string): boolean {
  return this.currentView() === view;
}
```

**Template:**
```html
<!-- Single class binding -->
<button [class.active]="isActive('home')">Home</button>

<!-- Multiple classes -->
<div [class.playing]="isPlaying()" [class.loading]="isLoading()">
  Content
</div>

<!-- ngClass object syntax -->
<button [ngClass]="{'btn-primary': isPrimary(), 'btn-secondary': !isPrimary()}">
  Click me
</button>
```

---

### 11. Conditional Content with *ngIf

**Template:**
```html
<!-- Simple condition -->
<div *ngIf="currentSong()">
  <h3>{{ currentSong()!.title }}</h3>
</div>

<!-- Else template -->
<div *ngIf="songs().length > 0; else noSongs">
  <!-- Show songs -->
</div>

<ng-template #noSongs>
  <p>No songs found</p>
</ng-template>

<!-- Multiple conditions -->
<div *ngIf="isPlaying(); else paused">
  Now playing...
</div>
<ng-template #paused>
  Paused
</ng-template>
```

---

### 12. Creating a Modal Form

**Component:**
```typescript
showCreateForm = signal(false);
newName = signal('');

toggleCreateForm(): void {
  this.showCreateForm.set(!this.showCreateForm());
  if (this.showCreateForm()) {
    this.newName.set('');
  }
}

submit(): void {
  if (this.newName().trim()) {
    // Handle submission
    this.toggleCreateForm();
  }
}
```

**Template:**
```html
<button (click)="toggleCreateForm()">➕ Create New</button>

<div class="modal" *ngIf="showCreateForm()">
  <div class="modal-content">
    <h3>Create New Item</h3>
    <input
      type="text"
      [(ngModel)]="newName"
      placeholder="Enter name"
      (keyup.enter)="submit()" />
    <div class="modal-actions">
      <button (click)="submit()">Create</button>
      <button (click)="toggleCreateForm()">Cancel</button>
    </div>
  </div>
</div>
```

---

### 13. Styling with ngClass and ngStyle

**Component:**
```typescript
getSongClass(song: ISong): object {
  return {
    'song-card': true,
    'featured': song.playCount > 100,
    'new': new Date(song.album.releaseDate).getFullYear() === new Date().getFullYear()
  };
}

getSongStyle(song: ISong): object {
  return {
    'border-color': song.isFavorite ? '#ff6b6b' : '#e0e0e0',
    'background-color': song.playCount > 50 ? '#f9f9f9' : 'white'
  };
}
```

**Template:**
```html
<!-- ngClass -->
<div [ngClass]="getSongClass(song)">
  {{ song.title }}
</div>

<!-- ngStyle -->
<div [ngStyle]="getSongStyle(song)">
  {{ song.artist.name }}
</div>

<!-- Inline -->
<div [class.playing]="song.id === currentSong()?.id">
  {{ song.title }}
</div>
```

---

### 14. Service Singleton Pattern

**Usage:**
```typescript
// When you inject a service, it's automatically a singleton
// (same instance across entire app)

export class SongListComponent implements OnInit {
  constructor(protected musicService: MusicService) {
    // musicService is the same instance everywhere
  }

  ngOnInit(): void {
    // All components share the same service state
    this.musicService.getSongs().subscribe(songs => {
      this.songs.set(songs);
    });
  }
}

export class PlayerComponent implements OnInit {
  constructor(protected musicService: MusicService) {
    // Same musicService instance as SongListComponent
  }

  ngOnInit(): void {
    this.musicService.currentSong$.subscribe(song => {
      this.currentSong.set(song);
    });
  }
}
```

---

### 15. Debugging Tips

**In Component:**
```typescript
// Log values
console.log('Current song:', this.currentSong());

// Debug signal changes
effect(() => {
  console.log('Search query changed:', this.searchQuery());
});

// Check subscriptions
this.musicService.getSongs().subscribe(
  songs => {
    console.log('Received songs:', songs);
    this.songs.set(songs);
  },
  error => console.error('Error:', error),
  () => console.log('Song subscription completed')
);
```

**In Template:**
```html
<!-- Display for debugging -->
<div style="border: 1px red dashed; padding: 10px;">
  DEBUG: {{ currentSong() | json }}
</div>

<!-- Check condition -->
{{ songs().length > 0 ? 'Has songs' : 'No songs' }}

<!-- Log in template (not recommended for production) -->
{{ console.log('Current view:', currentView()) || '' }}
```

---

## 🎯 Common Patterns

### Pattern 1: Signal-Based Form
```typescript
formData = signal({ name: '', email: '', password: '' });

updateField(field: string, value: string): void {
  this.formData.update(data => ({
    ...data,
    [field]: value
  }));
}

submit(): void {
  console.log('Form data:', this.formData());
}
```

### Pattern 2: Filtered List
```typescript
items = signal<Item[]>([]);
filterValue = signal('');

filteredItems = computed(() => {
  const filter = this.filterValue().toLowerCase();
  return this.items().filter(item =>
    item.name.toLowerCase().includes(filter)
  );
});
```

### Pattern 3: Tab Navigation
```typescript
activeTab = signal<'tab1' | 'tab2' | 'tab3'>('tab1');

selectTab(tab: 'tab1' | 'tab2' | 'tab3'): void {
  this.activeTab.set(tab);
}

isTabActive(tab: string): boolean {
  return this.activeTab() === tab;
}
```

---

**Happy Coding! 🚀**
