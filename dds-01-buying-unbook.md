__Buying an Un-Encyclopaedia__

_A Reality-based Example to Start with_

The Swedish bookstore Bokhuset put an order form online on their web-site. Of course, Bokhuset does not really exist, and the company in this story was in a completely different line of business. But to conceal their identiy, let us say that they were selling books.

The order form was the perfectly normal thing you would have expected. Browsing around the descriptions of books you could put 
them in a virtual shopping cart. When finished, you went to the checkout where you registered shipping address and paid using a credit card. 

For example, if you liked _Encyclopaedia of Application Security and Domain Driven Design_ which costs SEK 449, 
you might put that in your cart. If you wanted more that one, you could also change the quantity. To get four copies of 
the Encyclopaedia you changed the quantity in your cart and upon checkout your credit card would be billed SEK 1796.

One of the security testers probing the system got curious about the quantity field and started fiddling around with it. Trying arbitrary pieces of text gave various error messages on the theme "this is not an integer".

Inspired by this, in a creative moment the security tester tried changing the quantity to "-1". To her surprise there was no error message, neither when changing the quantity, nor when proceeding to checkout. Actually, the system seemed to accept the order completely. 

The day after they got a message from economy that their test customer had gotten a credit invoice on the amount of SEK 449 - as if the customer had returned a copy of the Encyclopaedia.

_What was Wrong_

We can just speculate in what was wrong under the hood. Apparently, the order system sent a transaction to the payment system, probably with the amount "SEK -449" which the payment system interpreted as an order to issue a credit invoice to the customer. Probably the shipping system got a request to pack "-1" Encyclopaedia, a request we can guess it discarded as flawed and dumped some error message on a seldom read log file. The inventory system might have gotten a message to decrease the stock of Encyclopaedias with -1 - which might have caused it to increase the stock with one.

The website order of one "un-Encyclopaedia" might have caused some subtle inconsistency within the business, but it is also possible that all systems would seem consistent to each other. The buying of un-books could have passed unnoticed until the next stock inventory of the storehouse.

_Application Security_

Nowadays it is not so common that crackers make their way into computer systems via the infrastructure. Routers, firewalls, and servers are nowadays pretty well configured in most enterprises. The new route in is via the applications that are exposed to the rest of the world. Now, these applications, like the online order system, cannot be hidden behind firewalls. The entire point of the online order system is that it is available to any customer at any point of time.

Application Security is concerned with ensuring that your application can not be used in a malicious way that put your enterprise at risk. Validating indata is an important part of this.

_Indata Validation and Domain Driven Design_

There is one thing that is often a little bit sloppily overlooked when talking about "indata validation". It is that the word "validation", or "valid", is pretty meaningless standing for itself. You have to explain in what sense the data is "valid". Obviously a valid phone number is something completely different than a valid shipping address.

But even telling which domain (phone or shipping) might not be enough. A phone number can be regarded as valid or invalid in so many different ways. If we want "valid indata" to mean something useful we need to be really precise.

For example we can say that a phone number must follow the format "0 followed by nine or ten digits, first of which may not be a zero" - something that obviously is even better expressed as a regular expression in some format, e g "0[1-9][0-9]^9[0-9]?".

What we have created here is actually a very precise, even mathematical, model of the concept "phone number" from the telephone domain. This is what in Domain Driven Design is called a domain model. Granted, the phone number is just a small part of such a domain model.

In the case of the Bokhuset example, the missing part was to spell out what a valid quantity looked like - that only positive integers makes sense. Most probably the program just accepted an integer.

_Code_

We can have a pretty good guess of what the code looked beneath the surface of the Bokhuset order system.

Random strings could not be entered as quantities. We can assume that the presentation tier is converting "4" which we type in the order form text to the integer 4 before changing the order.

Most probably there are somewhere a procedure to add an item to an order. We can assume there is some OrderServce class that face the presentation tier, providing the API to place orders

class OrderService {
	void addBook(String orderId, String productNumber, int quantity)
}

Most probably this method has a sister method for changing the quantity.

class Order {
	void changeQuantity(String orderId, String productNumber, int quantity)
}




 



