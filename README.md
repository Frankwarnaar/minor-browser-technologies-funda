# Funda - progressive enhancement wishlists

## What does my app do?
![Loading](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/loading.png)
My app requests the users location via javascript. Then it gets the streetname and city with the Google Maps API. This address is piped into the Funda api, to search houses within a kilomter from the users current street.

![Results](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/homescreen.png)
When the results come, the houses are rendered into an overview. 

![Detail](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/detailscreen.png)
If the user clicks on one results, he/she navigates to the detail page.

## Test lab
![Detail](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/screenshot_test-lab.png)

On one of the devices in the test lab the browser would not get the location of the user, because HTTPS was required. However, it would just keep on loading. So the app was totally useless.

## Improvements

### 1. Get results
#### Current process
Currently the next process takes place when the user enters the web app:
1. The app gets the coordinates of the user with the geo API.
2. These coördinates are passed to the Goole Maps API. This returns the matching address.
3. The street and city are passed to the Funda API. This returns houses in a radius of 1 kilometer of the given street and city.
If one of those steps don't succeed, the user gets a notifation that there couldn't be found any results.


#### Wished process
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

### 2. Thumbnails
![Thumbnails](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/detailscreen.png)
![Image preview](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/image.png)
When the user clicks a thumbnail in the detail page, the picture is being previewed big with javascript. How this should be done is:
1. Wrap the thumbnail in an anchor link. Link to a full screen image page.
2. With javascript, prevent the default behaviour of these anchor links.
3. Get the src of the big image.
4. Show a modal with the big image in it.

### 3. Screen reader / tabbing - sorting results
![Results](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/homescreen.png)
Currently when the user sorts the results, it's being reordered with the 'order' css property in Javascript. When the user tabs through results, while they are sorted, they are just visually sorted, not in the DOM. Besides that, this sorting doesn't work without javascript The way this should be done is with the next steps:
1. Set the sortable parameters as data attributes to the DOM element of each result.
2. Let the sort options link to the same page, with it's name of the sort option as a query string in the href. Keep the search query in the href.

-- without javascript --

3. Get the sort option from the query string server side. Get the results of the search query in the requested order from the funda API.

-- with javascript --

3. Prevent the default behaviour of the sort links. Get the name of selected sort option
4. Get all the houses from the DOM, with their parameters from their data attributes.
5. Sort the houses on the selected sort option
6. Remove the results from the DOM.
7. Render the sorted results again in the DOM.

### 4. Filters
![Filters](https://raw.githubusercontent.com/Frankwarnaar/minor-browser-technologies-funda/master/audits/homescreen.png)
Right now the filters are working with javascript. They should work like this:
1. Set the filterable parameters as data attributes to the DOM element of each result.
2. Let the filter options link to the same page, with it's name of the filter option as a query string in the href. Keep the search query in the href.

-- without javascript --

3. Get the filter option from the query string server side. Get the filtered results of the search query from the funda API.

-- with javascript --

3. Prevent the default behaviour of the filter links. Get the name of selected filter option
4. Get all the houses from the DOM, with their parameters from their data attributes.
5. Filter the array of houses on the selected filter option.
6. Remove the results from the DOM.
7. Render the filtered results again in the DOM.
