# Clients and servers

All computers connected to the internet have a unique **ip address** to identify them. Some special computers are **hosts** - they host websites on the internet. If we want to connect to a server on that host computer, we need to know it's ip address. Websites have **domain names** to mask the ip address of the host computer. 

When we type the domain name in the browser, it will look up the ip address of the computer hosting the website and connect to the server on the host computer with that ip address. The server will probably respond by sending back an HTML page. This client-server communication is done via **HTTP**. 

In node, we write code to manually create a server and listen for requests coming in from the browser. 



