**In one sentence:** REST over HTTP/JSON is a way for computer programs to send structured messages to each other over the internet — you address a message to a specific location, tell the recipient what you want, and get a reply with a status marker, similar to sending a letter through the postal system.

**The picture to hold:** Imagine you want to order something from a distant shop. You write a note describing what you want, put it in an envelope, address it to the shop with a clear destination (the URI), affix a stamp (the HTTP status code concept), and drop it in a mailbox. A postal worker carries your letter to the shop. The shop reads your note, fills your order, puts the result in another envelope with a delivery receipt, and sends it back. In REST, the "shop" is a server program, your note is an HTTP request, and the reply envelope contains JSON — a simple, text-based format for describing the order and the result.

### 0 · Kitchen table
The core idea: **You send a message to someone by addressing it, and they reply.** That's it. No jargon, no technical terms. If you can send a letter, you understand the whole picture. The "someone" is a computer program, the "message" is an HTTP request, and the "reply" is the HTTP response.

### 1 · Newcomer
Now we add the real names of things, always tied back to the letter picture.

- **URI (Uniform Resource Identifier)** — the address on the envelope. In the letter picture, this is the street address you write so the postal worker knows where to deliver. In REST, it's how the computer finds the specific program or resource you want. Example: `/v1/orders` is like writing "Acme Orders, 123 Main Street."
- **HTTP** — the postal service itself. The rules of how letters are picked up, carried, and delivered. It transports it from point A to point B.
- **JSON (JavaScript Object Notation)** — the content inside the envelope, written in a simple, readable text format. It looks like a miniature address book or order form: `{"item": "widget", "quantity": 3}`. JSON is verbose (it takes more words than a binary code would) but it's human-readable and works everywhere.
- **Status code** — a small note the postal worker adds to the delivery receipt, telling you what happened. `200` means "your letter arrived safely." `404` means "the address doesn't exist." `500` means "something went wrong at the destination." In the letter picture, this is like a postmark that says "Delivered," "Return to Sender," or "Area Not Served."
- **Method** — the type of action you write in the letter. `GET` is "merely read what's at the address." `POST` is "here's a new request, please add it." `PUT` is "update whatever is at this address." `DELETE` is "remove whatever is at this address." Think of these as different kinds of letters: a note asking for information, a form asking to add something, a change-of-address form, and a cancellation note.

One concrete example: You want to buy a widget from an online store. You send a `POST` request to the URI `/v1/orders` with a JSON body `{"item": "widget", "quantity": 1}`. The server processes it and sends back a `201 Created` status with a JSON reply containing your order number and delivery estimate.

### 2 · Practitioner
How the pieces fit together, why it was built this way, the first real trade-off.

- **Proper status codes** let you know what happened without reading the whole reply. If you get `204 No Content`, you know the action happened but there's nothing to show — like sending a change-of-address form and getting back a blank receipt.
- **Versioned URIs** (`/v1/orders`, `/v2/orders`) are like having different mail routes for different eras. Old letters still get delivered, but new letters go to the new route. This lets the shop change its system without breaking old orders that were already in the mail.
- **Idempotency keys for retries** solve the "did the letter arrive?" problem. If you send a letter and don't get a reply, you can write a special code on the second envelope: "If the first one already got through, overlook this." `PUT` and `DELETE` operations are idempotent — sending the same request twice has the same effect as sending it once, the same as sending two identical "cancel subscription" letters; the second one doesn't cancel it twice, it's already canceled.
- **Pagination** is needed because an envelope can only hold so much. If a shop has 10,000 order records, you can't fit them all in one letter. Instead, the server sends back a letter saying "Here are the first 50, and here's a link to the next 50." You flip the page and request the next batch, as with a magazine subscription that comes in monthly issues.

### 3 · Maintainer
Failure modes, what goes wrong, alternatives rejected and why, behaviour over time.

- **Over-fetching** is when the reply envelope contains far more information than you asked for. You wanted only the order number, but the server included the customer's full address, credit card checksum, and internal database IDs. It's like ordering a small milk and getting a whole cooler's worth — wasteful, and you have to throw most of it away.
- **Under-fetching** is when the reply doesn't have enough. You ask for the order details, but the server only gives you the order ID, and you need to send another letter to get the item names. It's like ordering a meal and getting only a fork — you're hungry and have to make another trip to the kitchen.
- **No built-in contract enforcement** means there's no official rulebook that both the sender and receiver must follow. You might agree verbally that "POST means create," but there's nothing stopping someone from using `POST` to mean "update." That's why we have OpenAPI — a separate document that spells out the exact rules, like a terms-of-service agreement for the postal route.
- **Streaming is awkward** because traditional email/letter sending is "all or nothing." You write the whole letter, stamp it, and send it. You can't say "here's the first page, and the next page will follow in a separate letter an hour later" without breaking the format. Real-time streaming protocols exist but they don't feel like regular REST letter-sending.

### Where the picture breaks
The postal analogy is helpful but has limits. Real networks can lose packets (a letter getting lost in the mail), have delays (cross-country mail takes days), and the "envelope" (JSON payload) has size limits — you can't cram a whole database row into a single letter. Also, the analogy suggests reliable delivery, but on the internet, delivery is best-effort; a `200 OK` status means the server *tried* to reply, not that the client *guaranteed* to receive it. The picture breaks if you imagine it as a perfect, guaranteed system — it's better to think of it as "best-effort delivery with return receipts."

### Words introduced here
- **URI** — an address that identifies a specific resource on the network.
- **HTTP** — the protocol (postal service rules) for sending messages between programs.
- **JSON** — a text format for structuring data inside a message; verbose but universally readable.
- **Status code** — a 3-digit number in the reply that tells you the outcome of the request.
- **Method** — `GET`, `POST`, `PUT`, `DELETE`; the "kind of letter" you send, indicating the action desired.
- **Idempotent** — an operation where sending it multiple times has the same effect as sending it once.
- **Over-fetching** — getting more data in the reply than you needed.
- **Under-fetching** — not getting enough data, requiring extra requests.
- **Pagination** — splitting a large reply into pages, each with a link to the next.
- **OpenAPI** — a formal contract document that describes the exact rules of the API, so both sides know what to expect.

### What I left out / what nobody knows
I left out the formal architectural constraints that define REST: client-server separation, uniform interface, statelessness, and cacheability as described in Fielding's dissertation. I also left out HATEOAS (links embedded in replies that tell the client what to do next) and many real-world deployment details (load balancers, DNS, TLS security, rate limiting).

What nobody knows: Even the people who design REST APIs sometimes disagree on the best way to handle edge cases. The "right" way is often a matter of convention and team agreement, not a physical law. The closest we get to certainty is the OpenAPI contract, which is only as good as the people who write and keep it up to date.