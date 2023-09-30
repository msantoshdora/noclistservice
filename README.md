# Problem Statement (https://homework.adhoc.team/noclist/)
Retrieve the NOC list
The Bureau of Adversarial Dossiers and Securely Encrypted Code (BADSEC) has asked you to retrieve a list of VIP users. Fortunately, BADSEC provides an API to the agency you've been working with. Unfortunately, it's not the best API in the world.

Your job is to write a program that securely and politely asks the BADSEC server for this list of users and prints it to stdout in JSON format.

As the server that your application will be hitting is not well written, you should seek to minimize the amount of communication it does. Furthermore, you should write a client that is resilient to errors in the server.

We'll want to compile your code from source and run it from a Unix-like command line, so please include the complete instructions for doing so in a COMMENTS file.

Output format
Your application should output a JSON-formatted list of user ids to stdout:

["9757263792576857988", "7789651288773276582", "16283886502782682407", "...etc"]
Your application should exit with a status code of zero on success, and nonzero on failure.

Log as much as you want to stderr, but make sure that stdout includes only the JSON output of your program if there is any; otherwise the results won't parse properly.

Running the server
Ensure that you have docker installed on your system.

The BADSEC server will run on port 8888. You can start it with:

docker run --rm -p 8888:8888 adhocteam/noclist

You should see Listening on http://0.0.0.0:8888.

If you have any trouble getting the server running, or any other questions about this homework, use the chat button in the lower right to contact us.

Handling errors
You may treat a dropped connection or any response code other than 200 as a failure.

Keep in mind that both the /auth and /users endpoints may fail.

If a call to an endpoint fails, you should retry up to 2 times. If the call fails 3 times in a row, your application should exit with a non-zero status code to indicate failure.

Avoid off by one errors! The correct sequence is: try -> fail -> retry -> fail -> retry -> exit if fail

Please write the retry logic yourself rather than use a request library with built-in retry functionality.

The server we've given you is just a demonstration server; it should not throw any errors.

The production server your code will run against after you submit it will return non-200 responses and randomly drop connections; your code should be able to handle those errors.

Server API
Authentication
The BADSEC server requires you to authenticate yourself by signing all requests with a provided auth token. To get the token, the first thing your application should do is call /auth. This endpoint will return a token you can use to generate the checksum for the subsequent user list call.

If you have any questions about authenticating, use the chat button in the lower right to contact us.

Checksum
To request the user list, you will need to provide a checksum in the X-Request-Checksum header. To calculate this checksum, you should compute:

sha256(<auth_token> + <request path>)
For example, if you had auth token 12345 and were requesting /users, the checksum would be:

sha256("12345/users") = c20acb14a3d3339b9e92daebb173e41379f9f2fad4aa6a6326a696bd90c67419
and you would include that value as the X-Request-Checksum header in your request.

The endpoints
GET, HEAD /auth
Before making further calls to the server, you must first call /auth to get an authentication token. The token will be valid until the server stops or issues another token.

The server will return the authentication token in the Badsec-Authentication-Token HTTP header.

Calling /auth will invalidate all previous auth tokens you have acquired, so make sure you use the most recent one.

Note that, for unclear reasons, this API call returns a large amount of useless data in the response body. You should ignore this useless data; you only need the data in the header.

GET /users
This endpoint will return a list of 64-bit newline-separated user IDs like this one:

9757263792576857988
7789651288773276582
1628388650278268240
Make sure you send the X-Request-Checksum header to the /users endpoint.

Included files
There are no included files to download for this assignment.

# Language Used
 Elixir version 1.13

# Steps to run NoclistService:
1. Clone repo in your local
2. Open terminal inside the noclistservice repo
3. Run command 'mix deps.get' to install the necessary dependencies
4. Run command 'source .env'
5. Run command 'iex -S mix' to open Interactive Elixir
6. Now enter command 'NoclistService.fetch_users' to run the code to fetch the NOC list users.

# Note
.env is the file to add the necessary ENVs for the URI to be called.
