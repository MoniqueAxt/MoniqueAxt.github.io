---
layout: page-fullwidth
title:  "Web Security Application"
subheadline:  "Projects"
teaser: "A project that focuses on the implementation of security measures in a web application."
categories:
    - portfolio
header:
   image_fullwidth: header_grey-security.jpg

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
      caption: Add/edit a location with autofill feature
    - image_url: gallery-android-fab.png 
      caption: FAB menu
    - image_url: gallery-android-delete.png
      caption: Delete confirmation
---

This application handles security vulnerabilities including SQL injection, XSS and XSRF. Inspiration from completion of the <a href="https://seedsecuritylabs.org/Labs_20.04/Web/">SEED labs.</a>
{% include gallery %}

<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->
<div class="medium-8 medium-pull-4 columns" markdown="1">

## About
A PHP/JavaScript web-based messaging application that provides a forum for users to communicate
by posting <em>Topics</em> or replying to these with <em>Posts</em>. Users need to be
logged-in to submit a Topic or Post. The latter can be upvoted, downvoted and deleted. 
The frontend was written in <a href="http://vanilla-js.com/">Vanilla JavaScript</a>, and while the website
and its associated forum are fully functional for the purposes of this project, the focus is on security.

## Website functionality
1. Users can post messages ⇔ they are logged in.
2. Users can up- and down-vote messages ⇔ they are logged in.
3. Search/filter posts by keyword.
4. Search/filter posts by username.
5. Users should be able to delete their own messages.

<div>
    <ul data-clearing>
        <li><a href="{{ site.urlimg }}php.jpg">
        <img src="{{ site.urlimg }}php-thumb.jpg" alt="notext">
</a></li>
    </ul>
</div>

## Environment and build
A PHP/JavaScript web application that was developed on a Linux/GNU distribution 
loaded on a virtual machine with a localhost server setup.
<em>Want a copy of the virtual machine? <a href="{{ site.url }}{{ site.baseurl }}/contact/">Contact me.</a></em>

| Setup
------ | ----- 
VM operating system     | Ubuntu (64-bit) 20.04.2 LTS   
IDE                     | PHPStorm 2020.1.4
Server hosting          | Apache/2.4.41
Database management     | PostgreSQL and PgAdmin 4
Version control         | BitBucket and git
Virtualization software | Oracle VirtualBox Manager 6.1
Languages used          | PHP 7.4.3, JavaScript, HTML5, CSS3
Documentation           | Doxygen 1.8.17
Coding tools            | JSHint

<small markdown="1">[Up to table of contents](#toc)</small>
{: .text-right }

## Main security measures
This sections details the security measures implemented. The overarching concepts
regarding security-related decision were based on the following principles:
1. Least privilege
2. Keep it simple
3. Defence in depth
4. Fail securely

### SQL injection
When an application communicates with a database,
it is important to safeguard against potential SQL injections. 

#### Prepared SQL statements
A <em>DatabaseConnection.php</em> class provides a method to query the database using prepared statements
(see Table 4). The key to this is that the SQL statements are performed in two separate steps: preparation
and execution. All queries to the database using parameter input are done using prepared statements.

#### Hiding sensitive DB data
In the case of an SQL-injection vulnerability leading to a breach, all passwords are hashed and verified using
`password_hash` and `password_verify.`

Additionally, the database password column data type is text (size limit of 1GB[^1]), ensuring there is enough
space to hold the hashed values.

[^1]: Text is variable unlimited length, although practically this is limited by the architecture. See <a href="https://wiki.postgresql.org/wiki/FAQ#What_is_the_maximum_size_for_a_row.2C_a_table.2C_and_a_database.3F">link</a>.

#### Limiting input size and type
While not as powerful as using prepared statements, limiting the size and type of input serves as some protection
against attacks by limiting options, AKA a sanity check. Checks are done both in the front and backend.

#### Principle of least privilege
Connecting to the database is done using an account with the least privileges necessary. This ensures that
in the case of a breach an attacker does not have full administrative capabilities and thus potential manipulation
of other assets in the database, or the server itself, is limited. In this application, the admin account
`postgres` has full privileges and a separate account `userbob` is used by the application to connect to
the database and execute queries

<small markdown="1">[Up to table of contents](#toc)</small>
{: .text-right }

### Web Server-Related

#### DOM-based XSS (type 0)
Insecurely written HTML pages are the prerequisite for this type of attack. In the CoolForums project, preventing
attacks that involve manipulating the DOM is done by limiting the use of JavaScript's `innerHTML()`
method (used only with trusted data, Table 14) and ensuring the use of methods such as `innerText()`
when the former method is not necessary - so that any malicious input is rendered as text. Additionally, all JS
construction of HTML elements is done using `createElement()`, setting its properties and then using `appendChild()`
(avoiding `insertAdjacentHTML()` for example) to inject the new element into the DOM.
Additionally, user-input fields are restricted in length on the client-side (a slight deterrent
as this can be altered) and on the server-side (see Limiting input size and type).

#### XSS type 1 & 2
AKA Reflected/Nonpersistent XSS (type 1) and Stored/Persistent XSS (type 2).
The potential risks are mitigated by validating input as well as HTML-encoding output before echoing it using `htmlentities()`
or `htmlspecialchars()` (see Table 15 & 16). A check is done in a PHP function
- if the topic ID and title do not match the data in the database, no data is returned (see Table 17).
  Data fetched from the database is always executed using prepared SQL statements.

In the case of a missed XSS bug, additional defensive measures were taken.
*HttpOnly Cookies*, which helps protect some users (if the browser used supports this) by preventing cookies
being read by the client using `document.cookie` (see Table 22).

*Double quotes* are used to wrap tag properties, which can help foil some attacks that can bypass HTML-encoding.

*A default character set* is set for the web pages, which helps reduce the potential attacker's options. This is
set in all web pages in the application, as encouraged by the HTML5 standard. (This could also be done via
PHP using `mb_internal_encoding("UTF-8")` or set as a `header()`, or within Apache2 config). This
measure is less important now than it was historically, as most modern browsers do not support UTF-7 (in order
to be compatible with the HTML5 specification).

#### HTTP Response Splitting
Whereas XSS attacks involve a malicious data inserted into the web page (as part of the HTML payload),
this vulnerability occurs when malicious input is inserted directly into the HTTP headers of the web page sent
to the victim. To defend against this, all headers set with PHP using the `header()` function are specified explicitly,
and not set using GET parameters that could be manipulated on the client-side.

#### XSRF 
Cross-site Request Forgery. The main steps taken to mitigate this attack:
1. POST-requests for write-operations, GET-requests only for read operations
2. Session timeout
3. A log-in token

GET requests are only used for operations that don't change state on the server-side and, additionally,
the parameters are validated and checked.
Erite operations are done using POST requests and this is checked on the back-end to restrict
form-processing to only POST type requests.

Another defence employed includes setting specific cookie attributes (see Table 22).
- The `SameSite` attribute specifies whether the cookie should be restricted to a first-party or same-site
context. Although the top browsers set "lax" as the default and GET requests in this project are limited,
it is still good practice to explicitly declare this attribute to limit the cookie to same-site requests.
- The `Secure` attribute instructs the browser to avoid sending cookies over an unencrypted HTTP request
(CoolForums was built with HTTPS in mind (see here) so setting this attribute will not cause issues).
- Setting the `Lifetime` attribute adds a timeout to the session which limits the window of opportunity for
an attacker.
- The `HttpOnly` flag is set to protect against XSS attacks by blocking access to a cookie from the client-
side, i.e. via the JavaScript `Document.cookie` API.

Additionally, a randomly generated session token is used to help thwart XSFR attacks. This token is created dynamically
by PHP and placed in the HTML of the webpage. Using JS, the token value is passed back to the server for validation.

<small markdown="1">[Up to table of contents](#toc)</small>
{: .text-right }

## Additional security measures
### HTTPS
As a standard defensive measure which mitigates against Man-In-The-Middle attacks, communication trans-fer is done
using via HTTPS, meaning data is protected by SLL/TLS encryption. For the purposes of the loc-ally contained
CoolForums application, the mkcert[^2] tool was used which is a simple tool for making locally-trusted development
certificates, so that a real environment could be emulated (without browser security warnings). Apache redirects for
the HTTP version of the URL as well as 404 or 50x errors were then configured.

[^2]: <a href="https://github.com/FiloSottile/mkcert">mkcert tool</a> 

### Obfuscation
Security via obscurity is the weakest form of security, but often requires very little effort to implement.
Related to the <em>Hiding sensitive database data</em> section, information leakage is a security 
vulnerability that provides an attacker with information that could lead to a breach of security or privacy policy.

*Avoiding verbose error/info messages.* 
Following crypto protocols for best practice, any information in this application that is sent from the
server to the client is minimized. Errors, for example, are logged in detail on the server; however the client is simply
aware that an error occurred and responds accordingly. Path information and version information are also obfuscated, although
more for learning purposes than as any real protection.

Moreover, default cases are included in the code logic, i.e. handling unexpected situations and input.


### Restricted access
Many of the website's features deal with restricted access. Using PHP's `header()` function, pages are redirected
if the user is not logged-in, denying access.



=================================================================

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



