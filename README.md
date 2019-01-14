
# IBM Client Developer Advocacy App Modernization Series

##  Small legacy JEE app for multiple labs in the App Modernization Series

This is a modified version of the code for [Getting started tutorial for Liberty](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial) for use in IBM Cloud Private developer training.

This is a minimal JEE app that requires the WebSphere Liberty Java application server and the Apache Derby  database to run.

## Required runtimes

**WebSphere Liberty Java application server:**  [WebSphere Liberty](https://developer.ibm.com/wasdev/websphere-liberty/) is a modular and easy-to-use Java application server, built on the open source [Open Liberty](https://openliberty.io/) project.

**Apache Derby:** [Apache Derby](https://db.apache.org/derby/) is an open source RDMS written entirely in Java.


## How it Works

The app consists  of a JEE web app that  prompts the user to enter their name. When a name is entered it  adds the name to a list stored in Apache Derby and then displays the entire contents of the database  

<p align="center">
  <kbd>
    <img src="images/GettingStarted.gif" width="300" style="1px solid" alt="Gif of the sample app contains a title that says, Welcome, a prompt asking the user to enter their name, and a list of the database contents which are the names Joe, Jane, and Bob. The user enters the name, Mary and the screen refreshes to display, Hello, Mary, I've added you to the database. The database contents listed are now Mary, Joe, Jane, and Bob.">
  </kbd>
</p>

This app adheres to best practices of the [12-Factor app](https://12factor.net/). A detailed checklist can be found [here](12-factor.md).
