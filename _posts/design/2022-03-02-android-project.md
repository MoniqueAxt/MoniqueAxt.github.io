---
layout: page-fullwidth
title:  "Android Java Application"
subheadline:  "Projects"
teaser: "More than a few days of work to demonstrate my 'can-stick-to-something-for-a-longer-period' ability."
categories:
    - design
header:
   image_fullwidth: header_unsplash_leaf.jpg

image:
    thumb: "gallery-example-1-thumb.jpg"
gallery:
    - image_url: gallery-android-saved-location.png
      caption: Display screen
    - image_url: gallery-android-map.png
      caption: Map feature
    - image_url: gallery-android-locations-list.png
      caption: A (database-syncronised) list of locations
    - image_url: gallery-android-search.png
      caption: Search function
    - image_url: gallery-android-swipe.png
      caption: Card swipe feature
    - image_url: gallery-android-add-new.png
      caption: Add/edit a location with auto-fill feature
    - image_url: gallery-android-fab.png 
      caption: FAB menu
    - image_url: gallery-android-delete.png
      caption: Delete confirmation
    

---
An app to save favourite or secret spots that may be "off the beaten path", encouraging exploration of nature.
While there are many improvements still to be made, it's one of the projects into which I've invested a fair bit of time.

{% include gallery %}

<div class="row">
<a href="{{ site.urlimg }}gallery-android-saved-location.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-saved-location.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Display saved locations</b>
The main function of the app is to store and display user-saved data. Custom views, Room Library, RecyclerViews, (Android) View Model and Adapter classes were used to achieve this. <a href="https://developers.google.com/maps/documentation/places/web-service/overview">Google Places API</a> displays a corresponding picture if possible, otherwise a default image is shown.

This fragment is reached via the user selecting 'view' on the <i>Locations</i> (home) screen, or it is automatically navigated to from the <i>Add New</i> screen after a location is saved, using the Navigation component. A (custom class) <i>location object</i> is sent via a Bundle and the fields are filled with this information. If possible, locations are saved in the database with a Places API code which is then used here to call a FetchPhotoRequest.

This screen also features a FAB-menu (<i>see FAB menu</i>) containing options to edit, delete or view the location on the map. The latter option navigates with a Bundle to the Map fragment.

<i>Improvements: user-uploaded pictures</i>
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-map.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-map.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Map</b>
The Map screen displays custom map markers representing saved locations within a specified radius (set in the settings). Tapping on a map marker's info-window results in navigation to the display fragment (<i>see Saved locations</i>).

An Android ViewModel is used to observe changes in the database and update accordingly.

A user can also reach this screen by using the <i>Card swipe</i> feature on the home <i>Locations</i> screen (<i>see Locations List</i>). When this happens, the map will move the camera to the swiped location.

<i>Improvements: improve available functionality when location permission is not granted</i>
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-locations-list.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-locations-list.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Locations List (the home screen)</b>
This displays the list of locations saved in the database. A Recycler View container is included in the layout of fragment and CardViews are used to display each item in the database.

The RecyclerViewAdapter is used for the search and swipe functionality to get an individual location (database row), and when observing with the associated ViewModel to fetch all database rows.

A user can also tap the image at the top of the screen (a custom view) which displays a location chosen randomly from the database. This is done via the associated ViewModel and using a query that requests a location ordered by random with a limit of 1.

When the application is first installed (determined by checking SharedPreferences), three example locations are created and saved. These quickly and effectively explain the functionality of this screen. That is, illustrating how a location will be displayed and indicating that each item can be clicked/tapped or swiped (<i>see Search</i>, <i>see Swipe</i>).

<i>Improvements: prettier cardview interface; option to change default home screen</i>
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-search.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-search.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Search (on home screen)</b>
A user can search for a location on the home screen using a SearchView.

A listener is set and the onQueryTextChange() and onQueryTextSubmit() methods are overwritten. The associated (extended) RecyclerViewAdapter filters the query using locations' address and name.

The Adapter class also contains an inner ViewHolderRow class that fills the CardHolder items -representing each location- with data. ViewHolderRow handles the onClick events and the transition-animation of the CardViews.

<i>Improvements: Extended search functionality; improved animation</i>
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-swipe.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-swipe.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Card Swipe (on home screen)</b>
Each item on the home screen can be swiped which reveals three buttons that can delete a location, navigate to the display fragment for viewing, or navigate to the Map screen (<i>See Saved Locations, see Map</i>).

The latter option will center the map's camera on the swiped location, regardless of the user's current location or radius setting.

Selecting delete will result in a confirmation dialogue. The same dialogue is displayed when a user chooses to delete <i>all</i> location data from the database (in settings).

The swipe functionality is achieved within an extended ItemTouchHelper class that uses GestureDetector, an inner UnderLayButtons class, MotionEvent and the RecyclerViewAdapter to get the swiped item's location in the RecyclerView.

<i>Improvements: prettier buttons</i>
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-add-new.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-add-new.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>Add / Edit location</b>
The Add New screen provides the ability to create or edit a location. Which mode to load is determined simply by checking getArguments() and setting a boolean variable depending on whether the former has data.

Navigation to this screen occurs either by manually selecting the option in the bottom-navigation panel, or automatically when a user chooses to edit a location (either via swiping on the home screen, or via the FAB-menu when viewing the location (<i>See Locations List, see Saved Locations</i>).  Like the other fragment classes, an associated AndroidViewModel is used to observe data changes in the database, and navigation is done using NavHostFragment with a Bundle.

In adding-new mode, the user is navigated to the display fragment (<i>see Saved Location</i>) with a Bundle containing the newly saved location, which is used to fill the fields. Each location is marked as unique by its coordinates and duplicates are not allowed. Before a location is saved, the database is queried against the provided coordinates. If the location already exists, the user is informed and offered the option of editing the already-saved location.

In editing mode, a Bundle containing the (location) object to edit is used to fill the form fields and an "Update" button is displayed instead of a "Save" button.
        </div>
    </div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-add-new2.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-add-new2.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>... continued ...</b>
Additionally, this page offers the cool feature of getting the details of the location, i.e. the current location of the user.

The <i>Get Current Place</i> button will automatically fill the current date, the location's name (depending on Place API), address and co-ordinates, and the weather (condition and temperature) at the location. The latter is requested from the <a href="https://openweathermap.org/api">OpenWeatherMap API</a> in a background task.

This feature can also be fined-tuned by using the similarly-colored buttons next to each of the aforementioned fields. Each of these buttons will only make the requests necessary to fill its corresponding field/s.

<i>Improvements: allow locations to be saved without coordinates; implement an alternative to AsyncTask for background database queries</i>
</div>
</div>
</div>

<div class="row">
<a href="{{ site.urlimg }}gallery-android-fab.png" target="_blank">
    <div class="medium-4 columns t30">
        <img src="{{ site.urlimg }}gallery-android-fab.png" alt="{{ site.urlimg }}placeholder-planning.jpg">
    </div>
</a>
    <div class="medium-8 columns t30">
        <div style="white-space: pre-wrap"><b>FAB Menu (on Display screen)</b>
A FloatingActionButton menu is implemented on the fragment used to display locations. This fragment shares functionality with the home screen (<i>see Location List</i>), i.e. to delete, view and view a location on the Map.

Accordingly, the display fragment uses the home's ViewModel in order to implement the FABs' functionality.

Animations are used to "open" and "close" the FAB menu along with modifying visibility of the menu buttons.
        </div>
    </div>
</div>



