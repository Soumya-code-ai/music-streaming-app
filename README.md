# 🎵 MusicStream - Professional Music Streaming App

A fully-featured, modern music streaming application built with **Angular 20+**, **TypeScript**, and **RxJS**. This project demonstrates advanced Angular concepts including signals, reactive programming, component composition, and state management.

## ✨ Features

### 1. **TypeScript Architecture**
- ✅ Interfaces for type safety: `ISong`, `IArtist`, `IAlbum`, `IPlaylist`
- ✅ Classes with inheritance and methods: `Song`, `Artist`, `Album`, `Playlist`
- ✅ Enums for structured data: `SongGenre`, `PlaylistType`
- ✅ Access modifiers and encapsulation

### 2. **Angular Components**
- **Navbar/Header** - Navigation with dynamic view switching (Home, Artists, Playlists, Now Playing)
- **Song List** - Display all songs with search, filter, play, favorite, and add-to-playlist options
- **Player** - Full-featured music player with progress bar, volume control, and playback controls
- **Playlist Manager** - Create, edit, delete playlists and manage songs within them
- **Artist Detail** - View artist information, biography, followers, and top tracks
- **Main App** - Layout orchestration and view routing

### 3. **Data Management**
- ✅ Centralized `MusicService` with RxJS Observables and Signals
- ✅ Sample data with realistic artists, albums, and songs
- ✅ Reactive state management with `BehaviorSubject`
- ✅ Signal-based state with Angular Signals API

### 4. **User Interactions**
- ✅ Play/Pause/Stop/Next/Previous controls
- ✅ Favorite songs toggle
- ✅ Add songs to playlists
- ✅ Search and filter by genre or query
- ✅ Create, edit, and delete playlists
- ✅ View artist details and top tracks
- ✅ Progress bar seeking and volume control

### 5. **Modern Angular Features**
- ✅ Standalone Components
- ✅ Angular Signals for reactive state
- ✅ Two-way binding with `[(ngModel)]`
- ✅ Structural Directives: `*ngIf`, `*ngFor`, `*ngSwitch`
- ✅ Property Binding: `[class]`, `[style]`, `[property]`
- ✅ Event Binding: `(click)`, `(change)`, `(input)`
- ✅ Template Syntax with computations

### 6. **Styling**
- ✅ Modern gradient designs
- ✅ Responsive grid layouts
- ✅ Mobile-first design (768px breakpoints)
- ✅ Smooth animations and transitions
- ✅ Professional color scheme

## 📁 Project Structure

```
src/app/
├── app.ts                          # Main app component with view routing
├── app.html                        # App template with conditional views
├── app.css                         # App styling
├── models/
│   └── index.ts                    # All TypeScript models, interfaces, enums
├── services/
│   └── music.service.ts            # Centralized music data service
└── components/
    ├── header/                     # Navigation navbar
    │   ├── header.ts
    │   ├── header.html
    │   └── header.css
    ├── song-list/                  # Song display and search
    │   ├── song-list.ts
    │   ├── song-list.html
    │   └── song-list.css
    ├── player/                     # Music player controls
    │   ├── player.ts
    │   ├── player.html
    │   └── player.css
    ├── playlist-manager/           # Playlist management
    │   ├── playlist-manager.ts
    │   ├── playlist-manager.html
    │   └── playlist-manager.css
    └── artist-detail/              # Artist information & tracks
        ├── artist-detail.ts
        ├── artist-detail.html
        └── artist-detail.css
```

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ and npm
- Angular CLI 20.3.9+

### Installation

1. Navigate to project directory:
```bash
cd music-streaming-app
```

2. Install dependencies:
```bash
npm install
```

3. Start development server:
```bash
ng serve
```
or
```bash
npm start
```

4. Open browser and navigate to:
```
http://localhost:4200/
```

## 📋 Component Details

### Header Component
- Navigation buttons: Home, Artists, Playlists, Now Playing
- Real-time display of favorite count and playlist count
- Signal-based state management
- Event output for view switching

### Song List Component
- Grid display of songs with album art
- Search functionality by title, artist, album
- Filter by genre
- Toggle favorite songs
- Play button for each song
- Add to playlist functionality
- Display play count and duration

### Player Component
- Album art display
- Current song information
- Progress bar with seek functionality
- Play/Pause/Stop/Next/Previous controls
- Volume control with mute button
- Time display (current/total)
- Animated play button

### Playlist Manager Component
- Sidebar with all playlists
- Create new playlists with name, description, and type
- Edit existing playlists
- Delete playlists
- Add/remove songs from playlists
- Display playlist statistics (song count, total duration)
- Type badge (Public/Private/Favorites)

### Artist Detail Component
- Artist sidebar with avatars
- Artist header with bio and stats
- Followers and song count display
- Top tracks list for selected artist
- Play and favorite functionality per track
- Genre and follower count display

## 🔧 Key Technologies

| Technology | Purpose |
|-----------|---------|
| **Angular 20** | Frontend framework |
| **TypeScript 5.9** | Type-safe JavaScript |
| **RxJS 7.8** | Reactive programming |
| **Angular Signals** | Modern state management |
| **CSS3** | Styling and animations |
| **Standalone Components** | Modern component pattern |

## 💡 Key Concepts Demonstrated

### 1. TypeScript Fundamentals
- Interfaces for contracts
- Classes with methods and properties
- Enums for constants
- Access modifiers (public, private, protected)
- Inheritance and composition

### 2. Angular Architecture
- Component composition
- Standalone components
- Dependency injection
- Service-based state management
- Two-way data binding

### 3. Reactive Programming
- RxJS Observables
- BehaviorSubject for state
- subscribe() pattern
- Observable composition

### 4. Modern Angular Features
- Signals API (`signal()`, `computed()`, `effect()`)
- Output API for component communication
- Computed properties for reactive UI
- Standalone components with imports array

### 5. Directives & Binding
```html
<!-- Structural Directives -->
<div *ngIf="condition"></div>
<div *ngFor="let item of items"></div>

<!-- Property Binding -->
<img [src]="song.coverUrl" />
<div [class.active]="isActive" />

<!-- Event Binding -->
<button (click)="playSong(song)"></button>

<!-- Two-way Binding -->
<input [(ngModel)]="searchQuery" />

<!-- Template Expressions -->
{{ song.title }}
{{ formatDuration(song.duration) }}
```

## 🎨 Styling Features

### Design System
- **Primary Gradient**: #667eea to #764ba2 (Purple)
- **Secondary Colors**: Red (#ff6b6b), Gray (#999)
- **Spacing**: Consistent rem-based sizing
- **Typography**: System font stack
- **Responsive Breakpoints**: 768px, 1024px

### Components Use
- Cards with shadows
- Gradient buttons and backgrounds
- Smooth transitions (0.3s ease)
- Hover states and animations
- Mobile-responsive grid layouts

## 📊 Sample Data

The app includes pre-loaded sample data with:
- **3 Artists**: The Weeknd, Dua Lipa, The Beatles
- **3 Albums**: After Hours, Future Nostalgia, Abbey Road
- **5 Sample Songs** with play counts
- **2 Default Playlists**: My Favorites, Chill Vibes

## 🧪 Testing

Run unit tests:
```bash
ng test
```

## 📦 Building for Production

Build the project:
```bash
ng build
```

The production build artifacts will be stored in the `dist/` directory.

## 📝 Code Examples

### Using the Music Service
```typescript
// Play a song
this.musicService.playSong(song);

// Toggle favorite
this.musicService.toggleFavoriteSong(songId);

// Create playlist
this.musicService.createPlaylist(name, description, type, imageUrl);

// Subscribe to songs
this.musicService.getSongs().subscribe(songs => {
  this.songs.set(songs);
});
```

### Signal-Based State
```typescript
// Define signal
protected songs = signal<ISong[]>([]);

// Update signal
this.songs.set(newSongs);

// Computed derived state
protected favoriteSongs = computed(() => 
  this.songs().filter(s => s.isFavorite)
);

// Use in template
{{ favoriteSongs().length }}
```

### Component Communication
```typescript
// Parent to child binding
<app-player [song]="currentSong()"></app-player>

// Child to parent event
<button (click)="viewChange.emit(view)"></button>

// Parent listening
<app-header (viewChange)="onViewChange($event)"></app-header>
```

## 🎯 Learning Objectives Met

✅ Angular environment setup with Node.js and Angular CLI
✅ TypeScript interfaces, classes, and enums
✅ Component modular architecture
✅ Data binding (property, event, two-way)
✅ Directives usage (*ngIf, *ngFor, [ngClass], [ngStyle])
✅ Service-based state management
✅ RxJS Observables and Signals
✅ Event handling and user interactions
✅ Responsive design with CSS Grid/Flexbox
✅ Professional component styling
✅ Production-ready code structure

## 🚀 Next Steps / Enhancements

Potential improvements:
- [ ] Backend API integration
- [ ] Authentication and user profiles
- [ ] Persistent storage (localStorage/Database)
- [ ] Audio playback with Web Audio API
- [ ] Sharing playlists
- [ ] User recommendations
- [ ] Dark/Light theme toggle
- [ ] Advanced search and filters
- [ ] Keyboard shortcuts
- [ ] Progressive Web App (PWA)

## 📄 License

This project is open source and available under the MIT License.

---

**Built with ❤️ using Angular 20, TypeScript, and modern web technologies.**

```bash
ng e2e
```

Angular CLI does not come with an end-to-end testing framework by default. You can choose one that suits your needs.

## Additional Resources

For more information on using the Angular CLI, including detailed command references, visit the [Angular CLI Overview and Command Reference](https://angular.dev/tools/cli) page.
