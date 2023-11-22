# Home - Owner

Date de création: 21 novembre 2023 11:00

# Functional test for the Home page Owner :

## Test 1 : Display

1. At the left of the page should have a banner with :
    1. In first in top left should have the logo and the name of the project (RamenAdvisor)
    2. Below the logo should have the list of restaurants. Click on a restaurant should change the middle area of the page with the informations of the restaurant
    3. At the end of the list should have an add button
    4. Below the list of restaurants should have the list of restaurants for comments
        1. Click on a restaurant should change the middle area and show the list of comments for this restaurant
    5. Below the list of comments should have 2 buttons : 
        1. edit profil button should redirect the user to the edit profil page
        2. Disconnect button which redirect the user to the connection page
2. In the middle of the page should have all informations about the first restaurant registered :
    1. At the left should have images of this restaurant
    2. At the right should have a map with the localisation of the restaurant
    3. Below this 2 should have the different inputs of the restaurant:
        1. name of the restaurant
        2. address
        3. City
        4. Phone number
        5. Web 
        6. Email
        7. Terasse
        8. If the restaurant have a disabled access
        9. Type food
        10. menu
    4. At the bottom should have a register button

## Test 2 : Add a restaurant

To test to a restaurant, we will use the following step :

1. Click on the button add 
2. Enter required informations of the restaurant
    1. Informations required should be : name of the restaurant, address, city, disabled access and type food. They should have a * next to their inputs
    2. Enter information separately and click on the register button
    3. If you do not enter the inputs for the name, address and city a pop up should appear. For the other an error message should appear
3. Make sur informations are valid
    1. phone number :
        1. Enter distinctly : letters, capital letters, different special characters and click on the register button. An error message should appear
        2. Only number should be accept 
    2. email information is valid when it compose to 3 parts :
        1. email information is valid when it compose to 3 parts :
            1. first part we can have letter, number, capital letters and this characters : /, +, -, _, *
            2. second part is an “@”
            3. third part need to be only letter and “.” (the point have to be follow by only letter)
        2. Enter different not valid email and check if an error message appear
            1. Enter a special character in the first part and click on the sign in button.
            2. Enter a text without @ and click on the sign in button.
            3. Enter a special character in the third part and click on the sign in button.
    3. address of the restaurant
        1. Enter a real address should update the map at the position of the address with a pin at this place
        2. Any character can be enter
4. If informations are not valid an error message should appear
5. Check in the database if the informations are correctly registered

## Test 3 : Change Information(s)

To test to change informations of a restaurant, we will use the following test :

1. Click on the restaurant in the list of restaurant on the left, the middle area should change like said in Test 1
2. Change informations in inputs
3. Make sur informations are valid
    1. Rules for valid informations are the same as the Test 2
    2. Only for address a message will be appear when the change is done
4. Check in database if the changes are correctly registered

## Test 4 : Comments

1. On the home owner page click on a restaurant in the restaurant list in comments section
2. The list of comments for this restaurant should appear