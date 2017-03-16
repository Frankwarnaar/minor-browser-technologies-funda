# Funda - progressive enhancement wishlists

## What does my app do?
![Loading](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/loading.png)
My app requests the users location via javascript. Then it gets the streetname and city with the Google Maps API. This address is piped into the Funda api, to search houses within a kilomter from the users current street.

![Results](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/homescreen.png)
When the results come, the houses are rendered into an overview. 

![Detail](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/detailscreen.png)
If the user clicks on one results, he/she navigates to the detail page.

## Improvements

### 1. Client side javascript
Currently the next process takes place when the user enters the web app:
1. The app gets the coordinates of the user with the geo API.
2. These coördinates are passed to the Goole Maps API. This returns the matching address.
3. The street and city are passed to the Funda API. This returns houses in a radius of 1 kilometer of the given street and city.
If one of those steps don't succeed, the user gets a notifation that there couldn't be found any results.

The wished process would be like this, to make the app work without javascript.
1. Show a search field, so the user can search for results. (On search, redirect to a page with the search query in the url. Skip to step 7)
2. Check if it's possible to get the users location with the geo API.
3. Get the users coördinates
4. Notify the user that it's possible to show houses within a range of one kilometer.
5. Redirect the user to a page with his coördinates as query params in the url.

-- Server side --

6. Pass the coördinates to the Google Maps Api.
7. Pass the street to the Funda Api.
8. Generate an Html page with the results. Set the house ID in the link of each house.
9. Serve the user the html page

-- Client side --

10. Click a house
11. The user get redirected to a page with the house ID as query string in the url.

-- Server side --

12. Get the ID of the house.
13. Handle a request to the Funda API to get the details of the house.
14. Generate an html page with the details.
15. Serve the user with the page
