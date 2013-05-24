__Buying an Un-Encyclopaedia__

_A Reality-based Example to Start with_

A Swedish business in the retail business put an order form online on their web-site. In reality this company was not selling books so, let us say that they were.

The order form was the perfectly normal thing you expected. Browsing around the descriptions of books you cut put them in a virtual shopping cart. When finished you went to the checkout where you registered shipping address and paid using a credit card. 

For example, if you liked Encyclopaedia of Application Security and Domain Driven Design which cost SEK 449 each, you might put that in your cart. If you wanted you could also change the quantity to something else then one. If you liked four copies of the Encyclopaedia you changed the quantity in your cart and upon checkout your credit card would be billed SEK 1796.

One of the security testers probing the system got curious about the quantity field and started fiddling around with it. Trying arbitrary pieces of text gave various error messages on the theme "this is not an integer".

Inspired by this, in a creative moment the security tester tried changing the quantity to "-1". To her surprise there was no error message, neither when changing the quantity, nor when proceeding to checkout. Actually, the system seemed to accept the order completely. 

The day after they got a message from economy that their test customer had gotten a credit invoice on the amount of SEK 449 - as if they had returned a copy of the Encyclopaedia.

_What was Wrong_

We can just speculate in what was wrong under the hood. Apparently, the order system sent a transaction to the payment system, probably with the amount "SEK -449" which the payment system interpreted as an order to issue a credit invoice to the customer. Probably the shipping system got a request to pack "-1" Encyclopaedia, a request we can guess it discarded as flawed and dumped some error message on a seldom read log file. The inventory system might have gotten a message to decrease the stock of Encyclopaedias with -1 - which might have caused it to increase the stock with one.

The website order of one "un-Encyclopaedia" might have caused some subtle inconsistency within the business, but it is also possible that all systems would seem consistent to each other. The buying of un-books could have passed unnoticed until the next stock inventory of the storehouse.

_Application Security_

Nowadays it is not so common that crackers make their way into computer systems via the infrastructure. Routers, firewalls, and servers are nowadays pretty well configured in most enterprises. The new route in is via the applications that are exposed to the rest of the world. Now, these applications, like the online order system, cannot be hidden behind firewalls. The entire point of the online order system is that it is available to any customer at any point of time.

Application Security is concerned with ensuring that your application can not be used in a malicious way that put your enterprise at risk. Validating indata is an important part of this.

_


 



