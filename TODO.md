# Elements

## Search

- Query text box
- Search button
- Also accepts enter to execute search

## Movie List

- Displays poster, title, year, and rating
- Can be sorted by title, year, rating by clicking the column heading. Sorts lexicographically on first click, then anti-lexicographically on the second click.
- Clicking on the poster or title links to the single-movie page
- Movies are sorted by title by default

## Poster List

- A list of movies
- Displays only the poster with the title below
- Clicking on either the poster or the title goes to the single-movie page.



## Movie Reference

- Maybe we should make this an actual object?
- Then we could have it automatically construct the URLs and what not.

```javascript
{
	id:
	tmdb_id:
	imdb_id:
	release_date:
	title:
	runtime:
	revenue:
	tagline:
	poster:
	overview:
	ratings: {
		popularity:
		average:
		count:
	}	
}
```



## Filter Object

This is a JavaScript thing for Clients. How do we transfer this to PHP?

It's here to stop a client from

```javascript
new Filter(title, year_between, rating_between)
// year_between is [min_year, max_year] inclusive
// rating_between is [min_rating, max_rating] inclusive
// to not filter by something, make it undefined

new Filter(undefined, [2000, undefined], undefined)
// This will get all the movies made in 2000 and newer.
```



# Other stuff to do

## favorites table

- [x] create SQL table for favorites



# APIs

## movies.php

`?id=`

3/10

- [ ] Return a list of [movies](#Movie-Reference) in JSON.







## db_helpers.php

8/10

### getDatabase

()

- [x] opens and returns a connection to the database
- [x] does the error checking for that



### runQuery

(database, sql, data)

- [x] does either a prepare statement or just a query statement.



### favoriteMovie

(id)

3/10

- [ ] returns true if successful or movie was already favorited
- [ ] returns false if there was some database error or something



### unfavoriteMovie

(id)

3/10

- [ ] returns true if successful or movie was not favorited
- [ ] returns false if there was some database error or something



### getFavoriteMovies

(email/userID)

3/10

- [ ] returns a list of movies favorited by a user



### getMovies

()

3/10

- [ ] returns a list of all the movies see [Movie Object](#Movie-Reference)



### registerUser

(first_name, last_name, city, country, email, password)

4/10

- [ ] checks that email doesn't already exist in Users database, if it does then returns "Email already taken"
- [ ] hashes the password
- [ ] adds the information to a new User row
- [ ] returns "Successfully registered" if successful
- [ ] otherwise returns some error string



### loginUser

(email, password)

4/10

- [ ] hashes password
- [ ] if email not in Users, return "Email invalid"
- [ ] if password not correct for email, return "Password invalid"
- [ ] return "Logged in" if successful
- [ ] otherwise returns some error string



### logout

()

1/10

- [ ] modifies session data to remove sessionID



### recommendations

(user_id)

6/10

That is, create a list containing:

• movies with the same release year as one of the favorited movies

• movies with the same (that is, within +/- 0.25) average rating as one of the favorited movies It’s possible that a user hasn’t favorited any movies yet; your algorithm must be able to handle this situation. In that case, show 10-15 of the most highly-rated moves (i.e., highest average rating). If your Movie You May Like algorithm has fewer than 10-15 images, fill it up with the most highly-rated movies



# Pages

## header.php

4/10

- This should be included in all the other pages. It should have a navigation bar and a logo
- The navigation bar:
  - Home `index.php`
  - About `about.php`
  - Search `browse-movies.php`
  - Favorites `favorites.php`
  - Login/Logout (changes depending on whether user is logged in) `login.php`/`logout.php`
  - Sign up (only if user isn't logged in) `register.php`
- CSS displays a flexbar on desktop, hamburger menu on mobile

## footer.php

1/10

closes the open body/html tags from header.php

## login.php ***Login***

`?last_page=`

6.5/10

- [ ] email text box
- [ ] password text box
- [ ] Login button:
  - [ ] [login_user](#login_user)(email, password)
  - [ ] Shows text returned by [login_user](#login_user)()
  - [ ] Save a sessionID as a cookie
  - [ ] Otherwise redirects to `last_page`, or `index.php`
- [ ] Register button:
  - [ ] Redirects to `register.php`



## logout.php ***Logout***

`?last_page=`

2/10

- [ ] run [logout](#logout)()
- [ ] displays "Logged out"



## register.php ***Register***

7.5/10

- [ ] first name field
- [ ] last name field
- [ ] city field
- [ ] country field
- [ ] email field (check to make sure it's an email address)
- [ ] password field
- [ ] confirm password field (has to be the same as password)
- [ ] sign up button:
  - [ ] [register_user](#register_user)(first_name, last_name, city, country, email, password)
  - [ ] displays the text returned by register_user
- [ ] Do we want this to redirect to some page?



## browse-movies.php ***Search***

`?q=`

8/10

- [ ] Extract search view from assignment 1
- [ ] Make the `q` query string set the value of the search query
- [ ] Ensure that the search view is complete (I don't remember if I finished all the functionality):
  - [ ] All the filters work as expected
- [ ] Make the view button redirect to `single-movie.php?id=`
- [ ] Make the movie list sortable by different columns:
  - [ ] Sort alphabetical and reverse alphabetical by clicking title column header
  - [ ] Sort lex and anti-lex by year
  - [ ] Sort lex and anti-lex by rating
- [ ] This page should use the [movies.php?id=](#APIs) API to get it's information
- [ ] Use event delegation for the movie list buttons
- [ ] Display a loading animation while the movies are fetched



## single-movie.php ***Movie Details***

4/10

- [ ] Extract details view from assignment 1
- [ ] This needs to access the database, **not** use the API (says the guidelines)
- [ ] Remove Speak and Close button
- [ ] Complete Crew and Cast information
- [ ] "Add to Favorites" button next to view button:
  - [ ] Button changes to "Remove from Favorites" if movie is in favorites (also update when the button is pressed)
  - [ ] Could also just have an empty and full heart icon
  - [ ] When button is pressed, use JavaScript fetch to `favorite-movie.php?id=`
  - [ ] When button is unpressed, use JavaScript fetch to `unfavorite-movie.php?id=`



## favorites.php ***Favorites***

6/10

### When not logged in

- [ ] Display an error message about not being logged in

### When logged in

- [ ] Display favorites; see [Movie List element](#Movie-List)
- [ ] Movies should be selectable with check boxes
- [ ] Select all button
- [ ] Remove from favorites button; removes all selected movies from favorites



## about.php ***About***

1.5/10

- [ ] Brief description of the website:
  - [ ] "Web II"
  - [ ] "Mount Royal University"
  - [ ] professor name
  - [ ] "Winter 2020"
  - [ ] technologies used
- [ ] Names and Github links for all group members



## index.php ***Home***

5/10

### When not logged in

- [ ] Login button
- [ ] Register button
- [ ] Search box
- [ ] Search submit button

### When logged in

- [ ] User info: country, city, email, first/last name...
- [ ] Search box & search submit button see [Search element](#Search)
- [ ] Favorite movies list; see [Poster List element](#Poster-List)
- [ ] Recommendations list; see [Poster List element](#Poster-List):
  - [ ] 
