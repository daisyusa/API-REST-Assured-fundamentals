In this lesson, we are going to take a look at some of the basic features and capabilities that REST Assured provides.
REST Assured provides support for all known HTTP methods. In the example in the previous chapter, we saw a HTTP GET, but REST Assured just as easily supports POST, PUT, PATCH, DELETE, and all other existing HTTP methods.

As we've seen in the previous chapter as well, REST Assured also supports the Gherkin syntax — the Given/ When/Then. Now, you either love or you hate the syntax. REST Assured doesn't force you to use it. It just gives you the option to do so.

To enable you to write checks in a human readable language, REST Assured uses Hamcrest Matchers. We'll see more about Hamcrest Matchers in a bit.

And it's also really useful to know that REST Assured uses JsonPath/GPath and XmlPath for selecting elements from response bodies.

#Checking the Response Status Code

So, a very basic first check that we would want to write in REST Assured is to check the response status code for an API call. You can do this very easily in REST Assured with the status code assertion method, which takes exactly one parameter, being the expected status code, as an integer.

Now, if we perform the same GET API call that we saw in the previous chapter and we put in an assertion on the status code being equal to 200 and we run our tests, we see that this test passes, telling us that the status code of the API response was indeed equal to 200.

import io.restassured.http.ContentType;
import org.junit.Test;

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class Chapter2Test {

    @Test
    public void requestUsZipCode90210_checkStatusCode_expectHttp200() {

        given().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            assertThat().
            statusCode(200);
    }
}
If we change to expected status code to 201 and we run our test again, we see that our test now fails, and it tells us that the expected status code was 201 but the actual status code was 200.

#Checking the Response Content Type
Another thing that we might be interested in is the content type that's specified in the response header. Now, this content type tells the consumer of the API how to actually decode the response that's being returned by the API provider, so this is a very important header. So, it makes sense to write a check for it and see if it matches the expected content type value.

    @Test
    public void requestUsZipCode90210_checkContentType_expectApplicationJson() {
        given().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            assertThat().
            contentType(ContentType.JSON);
    }
In REST Assured, we can assert on the content type using the content type method, which takes a single argument, specifying the expected content type. In this case, this would be JSON.

So, if we run our test, we see that this test passes, telling us that the actual content type returned was indeed application/json

Now, instead of using this enumeration, I could have also directly specified the string literal representation of this content type

contentType("application/json");
and the test would have passed just as well, but personally, I prefer using the enumeration.

If we put in XML instead

contentType(ContentType.XML);
and run our test, we can see that our test now fails, so indeed it asserts correctly on the expected content type.

#Logging Request and Response Details

Now, as you know, API requests and responses contain much more data than just a status code or a content type.

If you're interested to see what your REST Assured API test actually sends to the API provider or if you want to see everything that's being returned by the API provider when you put in a request, you can do so with the help of a number of logging statements.

    @Test
    public void requestUsZipCode90210_logRequestAndResponseDetails() {
        given().
            log().all().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            log().body();
    }
Now, in this test method, after the given, I specified log().all() and this tells REST Assured to write everything there is to know about the API request to the standard output.

I've done something similar after the then method, and this tells REST Assured to log the contents of the response body to the standard output as well.

So, let's see what happens if we run this test method.

Output

Request method: GET
Request URI:    http://zippopotam.us/us/90210
Proxy:          <none>
Request params: <none>
Query params:   <none>
Form params:    <none>
Path params:    <none>
Headers:        Accept=*/*
Cookies:        <none>
Multiparts:     <none>
Body:           <none>
{
    "post code": "90210",
    "country": "United States",
    "country abbreviation": "US",
    "places": [
        {
            "place name": "Beverly Hills",
            "longitude": "-118.4065",
            "state": "California",
            "state abbreviation": "CA",
            "latitude": "34.0901"
        }
    ]
}
What REST Assured now has done, it has written everything there is to know about the API request to the standard output, so you see the request method that's used, the resource or the URI that the request has been sent to, as well as any parameter values that have been used, header values, cookies, and more.

Because I've asked REST Assured to write the body of the API response to the standard output as well, you can find it in the console too.

And these logging statements can be very useful when you don't know exactly what's happening when you write or execute your REST Assured tests.

#Checking the Response Body

Now that we've seen how to write the response body to the standard output, let's write some actual checks for it.

We've seen an example of this in the previous chapter as well, but let's look into this test in a little more detail.

    @Test
    public void requestUsZipCode90210_checkPlaceNameInResponseBody_expectBeverlyHills() {
        given().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            assertThat().
            body("places[0].'place name'", equalTo("Beverly Hills"));
    }
After the then, which is the part of the REST Assured tests where I specify my expectations, I use the body method, which takes two arguments, to specify the assertion that I want to perform on the response body.

The first argument is a JsonPath expression, which points to the exact location in the API response of the element that I want to check. And the second argument is a Hamcrest matcher, which is an expression representing the expected value of the element.

In this specific test, I'm looking at the first entry in the list of places that returned by the API, and I want to extract the element called place name.

Now, in this case, the element name is enclosed by single quotes because there's a nasty space in it and if I omit those single quotes, the JsonPath expression with be incorrect, so that's why I need them here.

The Hamcrest Matcher expressing the check that I want to perform on that element value is a simple equalTo Hamcrest Matcher, which is a one on one comparison between the actual value of the elements that I've extracted using the JsonPath expression and the String value that I specified as an argument to the Hamcrest Matcher, and in this case, that's "Beverly Hills."

When I run this test again, something similar to what we've done in the previous chapter, we can see that this one passes as well.

#JsonPath and GPath
Let's look at JsonPath and GPath in a little more detail. Here, you can see the code of the test that we've just created and ran, as well as the response body of the API call that this test performed.

Now, again, in this test, we're interested in the value of the element place name, which is inside the list of places that the API returns. So, we're interested to see if the actual value of the place name element is equal to "Beverly Hills."

To extract that value from the API response body, we use this JsonPath statement — places[0].'place name' — which again, tells us to take the first element of the places array in the JSON body and extract the place name element out of the first entry of the places array.

Now, JsonPath is a path expression language of JSON. It's really similar to what XmlPath does for XML and what XPath does for HTML. One thing that's also very important to know is that REST Assured uses the GPath notation of the JsonPath query language.

This is different from a tool like SoapUI, which uses JsonPath as well, but it uses a slightly different syntax to query the actual JSON document for specific element values, so that can get a little confusing.

If you want to see some more examples about using JsonPath and the GPath notation, you can visit http://groovy-lang.org.

#Hamcrest Matchers

It also makes sense to tell you a little more about Hamcrest Matchers. We want to write an assertion that tells us whether the actual value of the place name element in the API response is equal to "Beverly Hills", and we can do that with the Hamcrest Matcher you see here.

Hamcrest Matchers allow you to express expectations or checks or verifications you want to perform in your automated tests in readable language.

Some examples,

equalTo(X) - used to check whether an actual element value is equal to a pre-specified expected element value

hasItem("value") - used to see whether a collection of elements contains a specific pre-specified item value

hasSize(3) - used to verify the actual number of elements in a collection

not(equalTo(X)) - inverts any given matcher that exists in the Hamcrest library

If you want to know more about Hamcrest, and especially the Java implementation of Hamcrest, and all the matchers that are available within the library, you can visit the documentation at http://hamcrest.org.

Let's look at a couple of examples of the use of JsonPath expressions and Hamcrest Matchers in this example here.

Instead of extracting the place name from the Json response, I want to extract the state, and that's as easy as pointing to the first entry in the list of places in the API response again, and instead of extracting the place name element, extracting the state element, and for the US zip code of 90210, this should be equal to "California".

   @Test
   public void requestUsZipCode90210_checkStateNameInResponseBody_expectCalifornia() 
   {
       given().
       when().
           get("http://zippopotam.us/us/90210").
       then().
           assertThat().
           body("places[0].state", equalTo("California"));
   }
Let's run the test to see what happens. And it looks like this expression works as expected.

Now, as we've seen before, there are other matchers that are very useful with REST Assured tests, other than the equalTo matcher. One example of that is the hasItem matcher, which we see here.

    @Test
    public void requestUsZipCode90210_checkListOfPlaceNamesInResponseBody_expectContainsBeverlyHills() 
    {
        given().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            assertThat().
            body("places.'place name'", hasItem("Beverly Hills"));
    }
This JsonPath statement is slightly different from the previous in that it doesn't point to a specific entry in the list of places that's returned by the API, but instead, this expression tells me to extract all of the place name elements in the list of places that's being returned.

So instead of a single element, we now get a collection of elements, as a result of this JsonPath expression, and now we want to see if that collection contains an item equal to "Beverly Hills," and we can do so using the hasItem matcher.

And this test passes as well.

To see if the hasItem and the JsonPath expression that we used really work as intended, let's invert the expected expression — not(hasItem("Beverly Hills")). As we've seen before, this is an easy way to invert any given Hamcrest Matcher.

Let's and run the test again.

What the test now does is see if within the list of place names, corresponding to the US zip code of 90210, there is no value equal to "Beverly Hills". And our test tells us that we expected a collection which doesn't contain Beverly Hills, but the API and the corresponding JsonPath expression returns a collection of place names with exactly one element in it, which is equal to "Beverly Hills," so the test fails.

As a final example of using Hamcrest Matchers, we're going to extract the collection of place names again from the API response, and instead of asserting whether or not there is an element in it which has a specified value, we are going to write an assertion on the number of elements that is in the collection. And as we've seen before, we can use the hasSize Hamcrest matcher for that.

    @Test
    public void requestUsZipCode90210_checkNumberOfPlaceNamesInResponseBody_expectOne() {
        given().
        when().
            get("http://zippopotam.us/us/90210").
        then().
            assertThat().
            body("places.'place name'", hasSize(1));
    }
This test verifies that when we request the list of places that correspond to the US zip code of 90210, that there's exactly one place name in the list of places that's being returned by the API.

And here too, we see that the test passes. As a final check, let's change the expected size of the list of place names associated with the zip code, and run the test again

body("places.'place name'", hasSize(2));
And as we expected, the test tells us that I expected a collection of size 2, but what I found is a collection with only a single element in it, so I'm going to fail the test, which is exactly what we wanted to do.
