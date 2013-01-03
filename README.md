# SoundCloud-Java-Library

Inofficial Java library, which simplifies the use of the official [SoundCloud Java API wrapper](https://github.com/soundcloud/java-api-wrapper).

> If you like the library and want to support my passion, feel free to make any amount of [donation](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=RNWX3MMZDU5W2).

## Dependencies

Note: You need to put the latest dependent libraries in your CLASSPATH before compiling and running codes:

* [java-api-wrapper-1.1.2-all.jar](https://github.com/soundcloud/java-api-wrapper/) by SoundCloud
* [google-gson-2.2.2](http://code.google.com/p/google-gson/) by Google
* [json-simple-1.1.1](http://code.google.com/p/json-simple/)

## Usage

Add the compiled library to your classpath, import the library and create a new instance of the wrapper with your [application](http://soundcloud.com/you/apps) and personal [login](https://soundcloud.com/login/forgot) data:

```
import de.voidplus.soundcloud.*;
``` 
```
SoundCloud soundcloud = new SoundCloud(
    "APP_CLIENT_ID",
    "APP_CLIENT_SECRET",
    "LOGIN_NAME",
    "LOGIN_PASS"
);
```

## API

The service of [Apigee](https://apigee.com/embed/console/soundcloud) is proper for learning and testing the official [SoundCloud API](http://developers.soundcloud.com/docs/api/reference). All API calls are mapped to REST methods:

```
public  get( String path [, String[] filters] )  <T>:T                // GET
public  put( String path [, Object value] )  <T>:T                    // PUT
public  post( String path, Object value )  <T>:T                      // POST
public  delete( String path )  Boolean                                // DELETE
```

## REST

You can use the following API calls and/or helpers ("→" ⇔ HTTP 303 See Other Redirect):

### GET

```
RESULT ───────────── METHOD ───────────────────────────────────────── API CALL ─────────────────────────
```

User:

```
User                 get("me")                                        // /me
                     getMe()

ArrayList<User>      get("users")                                     // /users
                     getUsers([Integer offset, Integer limit])

User                 get("users/{contact_id}")                        // /users/{contact_id}
                     getUser({contact_id})

ArrayList<User>      get("me/followings")                             // /me/followings
                     getMeFollowing([Integer offset, Integer limit])
User                 get("me/followings/{contact_id}")                // /me/followings/{contact_id}
                     getMeFollowing({contact_id})                     // → /users/{contact_id}

ArrayList<User>      get("me/followers")                              // /me/followers
                     getMeFollowers([Integer offset, Integer limit])
User                 get("me/followers/{contact_id}")                 // /me/followers/{contact_id}
                     getMeFollower({contact_id})                      // → /users/{contact_id}

ArrayList<User>      get("groups/{group_id}/users")                   // /groups/{group_id}/users
ArrayList<User>      get("groups/{group_id}/moderators")              // /groups/{group_id}/moderators
ArrayList<User>      get("groups/{group_id}/members")                 // /groups/{group_id}/members
ArrayList<User>      get("groups/{group_id}/contributors")            // /groups/{group_id}/contributors
                                                                      // 2271 = SoundCloud Sweetness ;)
```

Track:

```
Track                get("tracks/{track_id}")                         // /tracks/{track_id}
                     getTrack({track_id})

ArrayList<Track>     get("tracks")                                    // /tracks
                     getTracks([Integer offset, Integer limit])

ArrayList<Track>     get("me/tracks")                                 // /me/tracks
                     getMeTracks([Integer offset, Integer limit])

ArrayList<Track>     get("me/favorites")                              // /me/favorites
                     getMeFavorites([Integer offset, Integer limit])

ArrayList<Track>     get("groups/{group_id}/tracks")                  // /groups/{group_id}/tracks
                     getTracksFromGroup({group_id})

```

Playlist:

```
Playlist             get("playlists/{playlist_id}")                   // /playlists/{playlist_id}
                     getPlaylist({playlist_id})

ArrayList<Playlist>  get("playlists")                                 // /playlists
                     getPlaylists([Integer offset, Integer limit])

ArrayList<PlayList>  get("me/playlists")                              // /me/playlists
                     getMePlaylists([Integer offset, Integer limit])
```

Group:

```
Group                get("groups/{group_id}")                         // /groups/{group_id}
                     getGroup({group_id})

ArrayList<Group>     get("groups")                                    // /groups
                     getGroups([Integer offset, Integer limit])

ArrayList<Group>     get("me/groups")                                 // /me/groups
                     getMeGroups([Integer offset, Integer limit])
```

Comment:

```
ArrayList<Comment>   get("me/comments")                               // /me/comments
                     getMeComments([Integer offset, Integer limit])

ArrayList<Comment>   get("tracks/{track_id}/comments")                // /tracks/{track_id}/comments
                     getCommentsFromTrack({track_id})
```

### PUT

```
RESULT ───────────── METHOD ───────────────────────────────────────── API CALL ─────────────────────────
```

User:

```
User                 put("me", User user)                             // /me
                     putMe(User user)
```

Track:

```
Track                put("tracks/{track_id}", Track track)            // /tracks/{track_id}

Boolean              put("me/favorites/{track_id}")                   // /me/favorites/{track_id}
                     putFavoriteTrack({track_id})
```

### POST

```
RESULT ───────────── METHOD ───────────────────────────────────────── API CALL ─────────────────────────
```

Track:

```
Track                post("tracks", Track track))                     // /tracks
                     postTrack(Track track)
```

Comment:

```
Comment              post("tracks/{track_id}/comments", Comment comment)  // /tracks/{track_id}/comments
                     postCommentToTrack({track_id}, Comment comment)
```

### DELETE

```
RESULT ───────────── METHOD ───────────────────────────────────────── API CALL ─────────────────────────
```

User:

```
Boolean              delete("me/followings/{contact_id}")             // /me/followings/{contact_id}
Boolean              delete("users/{user_id}/followings/{contact_id}") // /users/{user_id}/followings/{contact_id}
```

Track:

```
Boolean              delete("tracks/{track_id}")                      // /tracks/{track_id}
                     deleteTrack({track_id})

Boolean              delete("me/favorites/{track_id}")                // /me/favorites/{track_id}
                     deleteFavoriteTrack({track_id})

Boolean              delete("users/{user_id}/favorites/{track_id}")   // /users/{user_id}/favorites/{track_id}
```

## Examples

### User & Me

Read user details:

```
User me = soundcloud.getMe();
System.out.println( me );

System.out.println( "ID: "+me.getId() );
System.out.println( "Username: "+me.getUsername() );
System.out.println( "Avatar-URL: "+me.getAvatarUrl() );

```

Update user details:

```
User me = soundcloud.getMe();
// OR
// me = soundcloud.get("me");

me.setCity("Berlin");
me.setCountry("Germany");
me.setDescription("Text of description.");
me.setWebsite("http://www.soundcloud.de/");
me.setWebsiteTitle("SoundCloud");

me = soundcloud.putMe(me); 
// OR
// me = soundcloud.put("me", me);

System.out.println( me );
```

Which sounds do you like? Show all your favorites:

```
User me = soundcloud.getMe();

Integer count = me.getPublicFavoritesCount();
Integer limit = 50; // = max
Integer pages = ((int)count/limit)+1;

ArrayList<Track> all_tracks = new ArrayList<Track>();

for(int i=0; i<pages; i++){
    ArrayList<Track> temp_tracks = soundcloud.getMeFavorites((i*limit),limit);
    // OR
    // ArrayList<Track> temp_tracks = soundcloud.get("me/favorites", new String[] {
    //     "order", "created_at",
    //     "offset", Integer.toString(i*limit),
    //     "limit", Integer.toString(limit)
    // });
    
    all_tracks.addAll(temp_tracks);
}
for(Track track : all_tracks){
    System.out.println( track.getTitle()+" (#"+track.getId()+")" );
}
```

Like ~ Add a new track to your favorites:

```
ArrayList<Track> tracks = soundcloud.getTracks(0,3);
// OR
// ArrayList<Track> tracks = soundcloud.get("tracks", new String[] {
//     "order", "created_at",
//     "offset", "0",
//     "limit", "3"
// });

Track track = tracks.get(0); // = ~random

Boolean add = soundcloud.putFavoriteTrack(track.getId());
// OR
// Boolean add = soundcloud.put("me/favorites/"+track.getId());
```

Unlike ~ Remove a track from your favorites:

```
Boolean removing = soundcloud.deleteFavoriteTrack(72688617);
// OR
// Boolean removing = soundcloud.delete("me/favorites/72688617")

if(removing){
    System.out.println( "Successful removing." );
}
```


### Track

Upload a new track:

```
Track track = soundcloud.postTrack(new Track("titel of the song", "path/to/file.mp3"));
// OR
// Track track = soundcloud.post("tracks", new Track("titel of the song!", "path/to/file.mp3"));

System.out.println( track.getTitle()+" (#"+track.getId()+")" );
```

Delete a track (Bad example, because we love your music!):

```
Boolean deletion = soundcloud.deleteTrack(track.getId());
// OR
// Boolean deletion = soundcloud.delete("tracks/"+track.getId());

if(deletion){
    System.out.println( "Successful deletion." );
}
```

List the last ten streamable tracks:

```
ArrayList<Track> streamable_tracks = soundcloud.get("tracks", new String[] {
    "order","created_at",
    "filter","streamable",
    "limit","10"
});

for(Track track : streamable_tracks){
    System.out.println( track.getTitle()+" (#"+track.getId()+")" );
}
```

### Group

Get the moderators of a group:

```
ArrayList<User> moderators = soundcloud.get("groups/2271/moderators");
for(User user : moderators){
    System.out.println( user.getId() );
}
```

### Comment

Post a new comment:

```
Comment comment = new Comment("Nice track!"); // new Comment("Nice track!", 120) // +timestamp

comment = soundcloud.postCommentToTrack(70734856, comment);
// OR
// comment = soundcloud.post("tracks/70734856/comments", comment);

System.out.println( comment );
```

## License

The library is Open Source Software released under the [MIT License](https://raw.github.com/voidplus/soundcloud-java-library/master/MIT-LICENSE.txt). It's developed by [Darius Morawiec](http://voidplus.de).