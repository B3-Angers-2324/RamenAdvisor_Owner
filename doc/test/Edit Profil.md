# Edit Profil

Date de création: 21 novembre 2023 11:00

# **Functional test for the Edit Profil page of Owner :**

## Test 1 : Display

1. The page Edit Profil should display 6 inputs :
    1. The last name of the Owner
    2. The first name
    3. The email
    4. adresse of the head office
    5. siret number
    6. name of the enterprise
2. The page should also display at the bottom 2 buttons :
    1. Register button
    2. Delete button

## Test 2 : Modify Informations

To test to modify the informations of an Owner profil, we will use the following test :

At the end of every step, click on the register button and check in database if the information you entered is correctly registered

1. Enter a new text in the last name input
    1. Any characters should be accept
2. Enter a new text in the first name input
    1. Any characters should be accept
3. Enter a new text in the email input
    1. email information is valid when it compose to 3 parts :
        1. first part we can have letter, number, maj letters and /, +, -, _, *
        2. second part is an “@”
        3. third part need to be only letter and “.” (the point have to be follow by only letter)
    2. Enter a not valid text and click on the register button. An error message should appear
4. Enter a new text in the head office address input
    1. Any characters should be accept
5. Enter a new number in the siret input 
    1. Enter distinctly : letters, capital letters, different special characters and click on the register button. An error message should appear
    2. Only number should be accept 
    3. Enter 14 number in the input and click on the register button. An error message should appear. Repeat this step with 16 message.

## Test 3 : Delete Profil

1. Find the owner profil in database
2. Click on the delete button
3. Check in database if the owner profil disappear