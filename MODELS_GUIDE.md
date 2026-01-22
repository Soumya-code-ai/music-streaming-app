/**
 * QUICK REFERENCE GUIDE - MusicStream TypeScript Models
 * 
 * This file demonstrates all TypeScript fundamentals used in the project
 */

// ============================================================================
// 1. ENUMS - Fixed set of constants
// ============================================================================

export enum SongGenre {
  POP = 'Pop',
  ROCK = 'Rock',
  HIP_HOP = 'Hip Hop',
  JAZZ = 'Jazz',
  CLASSICAL = 'Classical',
  ELECTRONIC = 'Electronic',
  R_AND_B = 'R&B',
  COUNTRY = 'Country'
}

export enum PlaylistType {
  PUBLIC = 'Public',
  PRIVATE = 'Private',
  FAVORITES = 'Favorites'
}

// ============================================================================
// 2. INTERFACES - Contracts defining structure
// ============================================================================

export interface IArtist {
  id: string;
  name: string;
  genre: SongGenre;
  bio: string;
  imageUrl: string;
  followers: number;
}

export interface IAlbum {
  id: string;
  title: string;
  artist: IArtist;
  releaseDate: Date;
  coverUrl: string;
  songs: ISong[];
}

export interface ISong {
  id: string;
  title: string;
  artist: IArtist;
  album: IAlbum;
  genre: SongGenre;
  duration: number;
  audioUrl: string;
  coverUrl: string;
  isFavorite: boolean;
  playCount: number;
}

export interface IPlaylist {
  id: string;
  name: string;
  description: string;
  type: PlaylistType;
  songs: ISong[];
  createdDate: Date;
  lastModified: Date;
  imageUrl: string;
}

// ============================================================================
// 3. CLASSES - Implementation of interfaces with methods
// ============================================================================

// Access Modifiers:
// - public: accessible everywhere (default)
// - private: accessible only inside the class
// - protected: accessible in class and subclasses

export class Artist implements IArtist {
  // Public properties (can be accessed from anywhere)
  public id: string;
  public name: string;
  public genre: SongGenre;
  public bio: string;
  public imageUrl: string;
  public followers: number;

  constructor(
    id: string,
    name: string,
    genre: SongGenre,
    bio: string,
    imageUrl: string,
    followers: number = 0
  ) {
    this.id = id;
    this.name = name;
    this.genre = genre;
    this.bio = bio;
    this.imageUrl = imageUrl;
    this.followers = followers;
  }
}

export class Album implements IAlbum {
  id: string;
  title: string;
  artist: IArtist;
  releaseDate: Date;
  coverUrl: string;
  songs: ISong[];

  constructor(
    id: string,
    title: string,
    artist: IArtist,
    releaseDate: Date,
    coverUrl: string,
    songs: ISong[] = []
  ) {
    this.id = id;
    this.title = title;
    this.artist = artist;
    this.releaseDate = releaseDate;
    this.coverUrl = coverUrl;
    this.songs = songs;
  }

  // Method: instance method that operates on album data
  getTotalDuration(): number {
    return this.songs.reduce((total, song) => total + song.duration, 0);
  }
}

export class Song implements ISong {
  id: string;
  title: string;
  artist: IArtist;
  album: IAlbum;
  genre: SongGenre;
  duration: number;
  audioUrl: string;
  coverUrl: string;
  isFavorite: boolean;
  playCount: number;

  constructor(
    id: string,
    title: string,
    artist: IArtist,
    album: IAlbum,
    genre: SongGenre,
    duration: number,
    audioUrl: string,
    coverUrl: string,
    isFavorite: boolean = false,
    playCount: number = 0
  ) {
    this.id = id;
    this.title = title;
    this.artist = artist;
    this.album = album;
    this.genre = genre;
    this.duration = duration;
    this.audioUrl = audioUrl;
    this.coverUrl = coverUrl;
    this.isFavorite = isFavorite;
    this.playCount = playCount;
  }

  // Methods for business logic
  toggleFavorite(): void {
    this.isFavorite = !this.isFavorite;
  }

  incrementPlayCount(): void {
    this.playCount++;
  }
}

export class Playlist implements IPlaylist {
  id: string;
  name: string;
  description: string;
  type: PlaylistType;
  songs: ISong[];
  createdDate: Date;
  lastModified: Date;
  imageUrl: string;

  constructor(
    id: string,
    name: string,
    description: string,
    type: PlaylistType,
    imageUrl: string,
    songs: ISong[] = [],
    createdDate: Date = new Date(),
    lastModified: Date = new Date()
  ) {
    this.id = id;
    this.name = name;
    this.description = description;
    this.type = type;
    this.imageUrl = imageUrl;
    this.songs = songs;
    this.createdDate = createdDate;
    this.lastModified = lastModified;
  }

  // Business methods
  addSong(song: ISong): boolean {
    if (!this.songs.find(s => s.id === song.id)) {
      this.songs.push(song);
      this.lastModified = new Date();
      return true;
    }
    return false;
  }

  removeSong(songId: string): boolean {
    const index = this.songs.findIndex(s => s.id === songId);
    if (index > -1) {
      this.songs.splice(index, 1);
      this.lastModified = new Date();
      return true;
    }
    return false;
  }

  getTotalDuration(): number {
    return this.songs.reduce((total, song) => total + song.duration, 0);
  }

  updatePlaylist(name: string, description: string): void {
    this.name = name;
    this.description = description;
    this.lastModified = new Date();
  }
}

// ============================================================================
// USAGE EXAMPLES
// ============================================================================

/*
// Create an artist
const artist = new Artist(
  'artist-1',
  'The Weeknd',
  SongGenre.R_AND_B,
  'Canadian singer known for atmospheric music.',
  'https://example.com/weeknd.jpg',
  1000000
);

// Create an album
const album = new Album(
  'album-1',
  'After Hours',
  artist,
  new Date('2020-03-20'),
  'https://example.com/after-hours.jpg'
);

// Create a song
const song = new Song(
  'song-1',
  'Blinding Lights',
  artist,
  album,
  SongGenre.R_AND_B,
  200,
  'https://example.com/blinding-lights.mp3',
  'https://example.com/blinding-lights.jpg'
);

// Use song methods
song.toggleFavorite(); // true
song.incrementPlayCount(); // playCount = 1

// Create a playlist
const playlist = new Playlist(
  'playlist-1',
  'My Favorites',
  'My favorite songs',
  PlaylistType.FAVORITES,
  'https://example.com/favorites.jpg'
);

// Use playlist methods
playlist.addSong(song); // true
const duration = playlist.getTotalDuration(); // 200 seconds
*/

// ============================================================================
// KEY CONCEPTS
// ============================================================================

/*
1. ENUMS: Define a set of named constants
   - Used for genres, playlist types, etc.
   - Type-safe alternatives to strings/numbers

2. INTERFACES: Define a contract/blueprint
   - What properties and methods should exist
   - No implementation, just signature
   - Used with 'implements' keyword

3. CLASSES: Actual implementation
   - Inherit from interfaces using 'implements'
   - Can have methods (functions)
   - Constructor for initialization
   - Access modifiers for encapsulation

4. INHERITANCE: Classes implement interfaces
   - 'implements' keyword shows contract fulfillment
   - Must implement all interface properties/methods

5. TYPE SAFETY: TypeScript catches errors at compile time
   - Variables have explicit types
   - Methods have return types
   - Parameters have expected types

6. ACCESS MODIFIERS:
   - public: everyone can access
   - private: only this class
   - protected: class and subclasses
*/
