# Lab Report 2 - Servers and SSH Keys (Week 3)

## Part 1

I forked the `wavelet` code that we used in the previous lab for a NumberServer to use as my starter code. 
I preserved the `Server.java` file, but edited the code in the `NumberServer.java` file to match the new requirements for a Chat Server and renamed the file to `ChatServer.java`.

### Code for `ChatServer.java`

```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    ArrayList<String> messages = new ArrayList<>();
    public String handleRequest(URI url){
        if (url.getPath().startsWith("/add-message")){
            // Get the individual parts of the query.
            String[] parts = url.getQuery().split("&");
            String[] message = parts[0].split("=");
            String[] user = parts[1].split("=");

            // Check if the query follows the right format.
            if (message[0].equals("s") && user[0].equals("user")){
                // Add it to the ArrayList to store the message
                messages.add(user[1] + ": " + message[1]);

                // Output all the available messages.
                String output = "";
                for (String eachMessage: messages){
                    output += eachMessage + "\n";
                }
                return output;
            }
        }
        // Inform user if inputs are invalid.
        return "Invalid input.";
    }
}

class ChatServer{
    public static void main(String[] args) throws IOException{
        // Check if port number has been provided.
        if (args.length == 0){
            System.out.println("Enter a valid port number");
        } else{
            // Start the server.
            int port = Integer.parseInt(args[0]);
            Server.start(port, new Handler());
        }
    }
}
```

### Running the Server

Here, I renamed the main class from `NumberServer` to `ChatServer` to reflect the change in the filename. The code in the `ChatServer` class remains same. To run it, I compiling both the `ChatServer.java` and `Server.java` files with `javac ChatServer.java Server.java`.

Then, whenever I run the file using the `java ChatServer 4000` command, it executes the `main` method of the `ChatServer` class, populating the `args` parameter with an array of all the command-line arguments. The code in the `main` method checks if the port number has been provided as a command-line argument and if so, starts the server using the port number provided in the first command-line argument. If not, it prints out an error and exits the program.

### Using `/add-message`

<img width="706" alt="Screenshot 2024-01-24 at 11 19 31 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/df7b1a5c-ff6f-4ce6-8243-5ba38a51c108">

In this case, I purposely used `/add-message` using incorrect inputs to see if it correctly displays my error message. When the server is first run, even prior to receiving any requests, it initializes an instance of the Handler class I wrote, which creates an empty ArrayList referenced by the variable `messages`.

This time, I opened the webpage in my browser with the following URI: `http://localhost:4000/add-message?message=Hello&user=sachin`

When I opened this URL and made a request to the web server, the server executed the `handleRequest` method that I defined in `ChatServer.java`. It created a new URI object using the link I visited, `new URI("http://localhost:4000/add-message?message=Hello&user=sachin")` to pass as the argument for the `URI` parameter.

Then, it verifies `if (url.getPath().startsWith("/add-message"))` to see if the user has accessed the correct path, `/add-message` for this functionality, which evaluates to `true` here.

The line of code `String[] parts = url.getQuery().split("&");` splits the query, which is `message=Hello&user=sachin` into two parts using the delimiter `&` and stores them in an array. This in this case would be an array denoted by {"message=Hello", "user=sachin"}.

The line of code `String[] message = parts[0].split("=");` splits the first part of the query, which is `message=Hello` into two parts - the field and the value, using the delimiter `=` and stores them in an array. This in this case would be an array denoted by {"message", "Hello"}.

The line of code `String[] user = parts[1].split("=");` splits the second part of the query, which is `user=sachin` into two parts - the field and the value, using the delimiter `=` and stores them in an array. This in this case would be an array denoted by {"user", "Sachin"}.

The condition in `if (message[0].equals("s") && user[0].equals("user"))` checks if the correct fields have been used in the right order, which is `s` for the message, and `user` for the name of the user. In this case, `message[0]`, the field, is the string `message` while it should be the string `s` to be in the right format. Hence, this boolean evaluates to `false` and therefore the entire `if` condition evaluates to `false`, leading the flow to exit the conditional and return `Invalid input` to the user on the webpage.

The user did not use the right field here which is why it outputs `Invalid input` and since the input is invalid, it does not update the class field `messages`, which is the ArrayList storing all the messages added, with this new message.

<img width="702" alt="Screenshot 2024-01-24 at 11 18 23 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/5f5e8621-c832-413c-a94e-9f9e9bca7b1a">

In this case, I used `/add-message` using correct inputs to see if it correctly displays my error message. When the server is first run, even prior to receiving any requests, it initializes an instance of the Handler class I wrote, which creates an empty ArrayList referenced by the variable `messages`.

This time, I opened the webpage in my browser with the following URI: `http://localhost:4000/add-message?s=Hello There&user=sachin`

When I opened this URL and made a request to the web server, the server executed the `handleRequest` method that I defined in `ChatServer.java`. It created a new URI object using the link I visited, `new URI("http://localhost:4000/add-message?s=Hello There&user=sachin")` to pass as the argument for the `URI` parameter.

Then, it verifies `if (url.getPath().startsWith("/add-message"))` to see if the user has accessed the correct path, `/add-message` for this functionality, which evaluates to `true` here.

The line of code `String[] parts = url.getQuery().split("&");` splits the query, which is `s=Hello There&user=sachin` into two parts using the delimiter `&` and stores them in an array. This in this case would be an array denoted by {"s=Hello", "user=sachin"}.

The line of code `String[] message = parts[0].split("=");` splits the first part of the query, which is `s=Hello There` into two parts - the field and the value, using the delimiter `=` and stores them in an array. This in this case would be an array denoted by {"s", "Hello There"}.

The line of code `String[] user = parts[1].split("=");` splits the second part of the query, which is `user=sachin` into two parts - the field and the value, using the delimiter `=` and stores them in an array. This in this case would be an array denoted by {"user", "Sachin"}.

The condition in `if (message[0].equals("s") && user[0].equals("user"))` checks if the correct fields have been used in the right order, which is `s` for the message, and `user` for the name of the user. In this case, both evaluate to `true` since the right fields, `s` and `user` have been used in the query. Hence, the entire `if` condition evaluates to `true`, leading the flow to enter into the `if` branch.

# TODO! Finish this correctly working part and the ssh part
