# Sasto Tickets Web-SDK Integration

## Introduction

The Web SDK allows you to integrate the service provided by Sasto Ticket into your web application. This document provided an overview of the SDK, how to initialize it, and how to handle success and failure scenarios.

## API Documentation

For more detailed information about the API integration, please refer to the [API Documentation](https://sastotickets-integration-b2b.web.app). This documentation provides in-depth explanations and examples of the API endpoints and their usage.

## Initialization

To initializxe the Web SDK, you need to open a specic URL with additional parameters encoded in Base64.

The SDK initialization URL should follow the format:

    https://demo.sastotickets.com/?data=<base64_encoded_parameters>

### Parameters

The parameters should be encoded in Base64 format and contain the necessary information for the SDK. Here's an example of the encoded parameter structure:

    {
        "access_token": "<your_access_token>",
        "phone_number" : "9801234567",
        "wallet_balance": 2000,
        "app_theme":"System",
        "success_url":"https://domain.com/success",
        "failure_url":"https://domain.com/failure"
    }

Encode the entire JSON structure in Base64, including the access token, and replace `<base64_encoded_parameters>` in the initialization URL with the actual encoded parameters.

<div style="background-color: #E2F0F7; padding:10px; border-radius:5px "><b>Note:</b> Replace &ltyour_access_token> with the actual access token you obtain by following the instructions in the API documentation.</div>

<div style="background-color: #E2F0F7; padding:10px; border-radius:5px; margin-top: 25px; "><p><b>Note:</b> When implementing for web, make sure to provide valid success_url and failure_url values. These URLs will be used for redirection upon success or failure of the SDK integration.</p>

<p>For Android and iOS implementations, success_url and failure_url can be left as null or omitted from the JSON structure. In such cases, the SDK will redirect the user to our default domain for success or failure.</p></div>

<div style="background-color: #E2F0F7; padding:10px; border-radius:5px;  margin-top: 25px; "><p><b>Note:</b> The app_theme field is optional and can be left as null or omitted from the JSON structure. In such cases, the system theme will be used by default.</div>

## Flight Booking & Ticketing Workflow

The flight booking and ticketing worl=kflow consists of the following steps:

    Step 1. Flight Search
    Step 2. Availability
    Step 3. Reservation
    Step 4. Payment
    Step 5. Issue Ticket

Web-SDK will handle the `1. Flight Search`, `2. Availability` & `3. Reservation` part and the `4. Payment` and `5. Issue Ticket` should be implemented at the your end.

After the completion of `3. Reservation` Web-SDK will redirct to the success url with necessary payload required to call `5. Issue Ticket` API

## Success and Failure Handling

After the SDK completes its task, it will redirect the use to the specified success or failure url provided in the encoded data of the parameter.

### Success URL

The success URL is the web page where the user will be redirected upon a successful completion of the flight booking and ticketing workflow. The success URL will include a payload encoded in Base64 as a query parameter. Ensure that the success URL can receive and process this payload.

###### Payload

        https://domain.com/success?data=<base64_encoded_payload>

In the example, the success URL includes a query parameter called data, which contains Base-64 encoded payload.

Here's an example of the encoded payload structure for the 

=== "<b>Oneway</b>"
        {
        "totalCost": 15384,
        "bearerToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUz",
        "flightsSummary": {
            "departureFlight": {
                "carrierCode": "GUA",
                "carrierLogo": "https://sastotickets-uat-flights.s3.ap-southeast-1.amazonaws.com/airlines/1660816647-1480147710.png",
                "carrierName": "Guna Airlines",
                "deptDate": "2023-01-01",
                "deptTime": "08:00 AM",
                "arrivalDate": "2023-01-01",
                "arrivalTime": "08:23 AM",
                "departureAirportCode": "KTM",
                "departureAirport": "Tribhuvan Int’l Airport",
                "departureCity": "Kathmandu",
                "departureCountry": "Nepal",
                "arrivalAirportCode": "PKR",
                "arrivalAirport": "Pokhara Airport",
                "arrivalCity": "Pokhara",
                "arrivalCountry": "Nepal"
            },
            "returnFlight": "null"
        },
        "emergencyContact": {
            "contact": "9849123456",
            "email": "info@sastotickets.com",
            "name": "SastoTicket"
        },
        "bookingSummary": {
            "PNR": "BNY5SO",
            "currency": "NPR",
            "fareInformation": {
                "cashBack": 0,
                "total_adt": 74635,
                "total_chd": 0,
                "total_cost": 74635,
                "total_inf": 0
            },
            "passengers": [
                {
                    "passenger_id": "ST2785",
                    "passenger_name": "Gurung Sujan",
                    "passenger_type": "ADT"
                },
                {
                    "passenger_id": "ST2786",
                    "passenger_name": "Thapa Bardan",
                    "passenger_type": "ADT"
                },
                {
                    "passenger_id": "ST2787",
                    "passenger_name": "Hitang Sonam",
                    "passenger_type": "CHD"
                }
            ],
            "bookingReferenceID": "5f982584-4ece-4b6c-9c9b-370964e1318f",
            "tripType": "oneway"
        }
    }

=== "<b>Round Trip</b>"

        {
            "totalCost": 15384,
            "bearerToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUz",
            "flightsSummary": {
                "departureFlight": {
                    "carrierCode": "GUA",
                    "carrierLogo": "https://sastotickets-uat-flights.s3.ap-southeast-1.amazonaws.com/airlines/1660816647-1480147710.png",
                    "carrierName": "Guna Airlines",
                    "deptDate": "2023-01-01",
                    "deptTime": "08:00 AM",
                    "arrivalDate": "2023-01-01",
                    "arrivalTime": "08:23 AM",
                    "departureAirportCode": "KTM",
                    "departureAirport": "Tribhuvan Int’l Airport",
                    "departureCity": "Kathmandu",
                    "departureCountry": "Nepal",
                    "arrivalAirportCode": "PKR",
                    "arrivalAirport": "Pokhara Airport",
                    "arrivalCity": "Pokhara",
                    "arrivalCountry": "Nepal"
                },
                "returnFlight": {
                    "carrierCode": "GUA",
                    "carrierLogo": "https://sastotickets-uat-flights.s3.ap-southeast-1.amazonaws.com/airlines1660816647-1480147710.png",
                    "carrierName": "Guna Airlines",
                    "deptDate": "2023-01-01",
                    "deptTime": "08:00 AM",
                    "arrivalDate": "2023-01-01",
                    "arrivalTime": "08:23 AM",
                    "departureAirportCode": "KTM",
                    "departureAirport": "Tribhuvan Int’l Airport",
                    "departureCity": "Kathmandu",
                    "departureCountry": "Nepal",
                    "arrivalAirportCode": "PKR",
                    "arrivalAirport": "Pokhara Airport",
                    "arrivalCity": "Pokhara",
                    "arrivalCountry": "Nepal"
                }
            },
            "emergencyContact": {
                "contact": "9849123456",
                "email": "info@sastotickets.com",
                "name": "SastoTicket"
            },
            "bookingSummary": {
                "PNR": "BNY5SO",
                "currency": "NPR",
                "fareInformation": {
                    "cashBack": 0,
                    "total_adt": 74635,
                    "total_chd": 0,
                    "total_cost": 74635,
                    "total_inf": 0
                },
                "passengers": [
                    {
                        "passenger_id": "ST2785",
                        "passenger_name": "Gurung Sujan",
                        "passenger_type": "ADT"
                    },
                    {
                        "passenger_id": "ST2786",
                        "passenger_name": "Thapa Bardan",
                        "passenger_type": "ADT"
                    },
                    {
                        "passenger_id": "ST2787",
                        "passenger_name": "Hitang Sonam",
                        "passenger_type": "CHD"
                    }
                ],
                "bookingReferenceID": "5f982584-4ece-4b6c-9c9b-370964e1318f",
                "tripType": "round"
            }
        }

### Failure URL

The failure URL is the web page where the user will be redirected if any error occurs during the flight booking and ticketing process. Similar to the success URL, ensure that the failure URL will receive any relevent error information as shown below

###### Example

     https://domain.com/failure?message="Unexpected Error"

In the example, the failure URL includes a query parameter called message, which contains the relevent message for the error.
