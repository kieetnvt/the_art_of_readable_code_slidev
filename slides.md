---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## The Art of Readable Code
  Presentation for developers.

drawings:
  persist: false
transition: fade-out
title: Welcome to Techtalk
---

# The Art of Readable Code

Code Should Be Easy to Understand

<div class="pt-12">
</div>

<div class="abs-br m-6 flex gap-2">
  NUS Technology, 16 Aug 2023
</div>

---
layout: default
---

# Packing Information into Names

![animal dangerous](/images/1.png)

<style type="text/css">
  img {
    max-width: 400px;
    margin: 0 auto;
  }
</style>

---
transition: fade-out
---

# Choosing specific words

Take a look on code below:

```ts{1}
def GetPage(url)
  ....
end
```

- The word “get” doesn’t really say much

- Does this method get a page from a local cache, from
a database or from the Internet?

- `FetchPage()` or `DownloadPage()`

<div class="abs-br m-6 flex gap-2">
  It’s better to be clear and precise than to be cute.
</div>

---
layout: default
---

# Finding More “Colorful” Words

| Word  | Alternatives |
| ----- | ------------ |
| send  | deliver, dispatch, announce, distribute, route |
| find  | search, extract, locate, recover |
| start | launch, create, begin, open |
| make  | create, set up, build, generate, compose, add, new |

<div class="abs-br m-6 flex gap-2">
  It’s better to be clear and precise than to be cute.
</div>

---
layout: default
---

# Avoid Generic Names Like tmp and retval

```ruby
  def generate_pdf
    tmp = ClientProgramSessionsController.render(
      'billing_details',
      layout: 'simple',
    )
    tmp2 = Grover::HTMLPreprocessor.process(tmp, @url, 'http')
    options = { printBackground: true, displayHeaderFooter: true }
    retval = Grover.new(tmp2, options).to_pdf
    @save_path = "/public/invoice_#{@user.id}.pdf"
    f = File.new(@save_path, 'w:ASCII-8BIT')
    f.write(retval)
    f.path
  end
```

- The name retval doesn’t pack much information. Instead, use a name that describes
the variable’s value

- The name tmp should be used only in cases when being short-lived and temporary
is the most important fact about that variable.

---
layout: default
---

# Avoid Generic Names Like tmp and retval

```ruby
  def generate_pdf
    html_string_rendered = ClientProgramSessionsController.render(
      'billing_details',
      layout: 'simple',
    )
    html_absoluted = Grover::HTMLPreprocessor.process(html_string_rendered, @url, 'http')
    options = { printBackground: true, displayHeaderFooter: true }
    pdf_raw_content = Grover.new(html_absoluted, options).to_pdf
    @save_path = "/public/invoice_#{@user.id}.pdf"
    f = File.new(@save_path, 'w:ASCII-8BIT')
    f.write(pdf_raw_content)
    f.path
  end
```

<div class="abs-br m-6 flex gap-2">
  If you’re going to use a generic name, have a good reason for doing so.
</div>

---
layout: default
---

# Attaching extra information to a name, by using a suffix or prefix

```ts
var start = (new Date()).getTime(); // top of the page
...
var elapsed = (new Date()).getTime() - start; // bottom of the page

document.writeln("Load time was: " + elapsed + " seconds");
```

- There is nothing obviously wrong with this code, but it doesn’t work, because getTime() returns
milliseconds, not seconds

- By appending `_ms` to our variables, we can make everything more explicit:


```ts
var start_ms = (new Date()).getTime(); // top of the page
...
var elapsed_ms = (new Date()).getTime() - start_ms; // bottom of the page

document.writeln("Load time was: " + elapsed_ms / 1000 + " seconds");
```

---
layout: default
---

# Attaching extra information to a name, by using a suffix or prefix

| Normal Name | Refactor Colorful Name | Easy understading for using variable    |
| ----------- | ---------------------- | --------------------------------------- |
| delay       | delay_secs             | delay is seconds not minute or hour     |
| size        | size_mb                | size is megabyte for easy to calculator |
| max         | max_kbps               | max kilobizes for instance              |
| name        | plaintext_name         | name is define plaintext for save to DB |
| html        | html_utf8              | html is encoding with utf8              |
| data        | data_int               | data is a integer type to save DB       |

---
layout: default
---

# Encoding Other Important Attributes

| Situation | Variable name | Better name |
| --------- | ------------- | ----------- |
| A password is in “plaintext” and should be encrypted before further processing | password | plaintext_password |
| A user-provided comment that needs escaping before being displayed | comment | unescaped_comment |
| Bytes of html have been converted to UTF-8 | html | html_utf8 |
| Incoming data has been “url encoded” | data | data_urlenc |

---
layout: default
---

# Bonus: Hungarian notation

- is a system of naming used widely inside Microsoft.

- It encodes the “type” of every variable into the name’s prefix. Here are some examples:

--  lAccountNum : variable is a long integer ("l");

-- arru8NumberList : variable is an array of unsigned 8-bit integers ("arru8");

-- bReadLine : function with a byte-value return code.

-- strName : Variable represents a string ("str") containing the name, but does not specify how that string is implemented.

---
layout: default
---

# Names That Can’t Be Misconstrued

Example: Filter()

```ruby
results = Database.all_objects.filter("year <= 2011")
```

What does results now contain?

- Objects whose year is <= 2011?

- Objects whose year is not <= 2011?

The problem is that filter is an ambiguous word.

It’s unclear whether it means “to pick out” or “to get rid of.

If you want “to pick out,” a better name is `select()`. If you want “to get rid of,” a better name
is `exclude()`.

---
layout: default
---

# Prefer min and max for (Inclusive) Limits

```python
CART_TOO_BIG_LIMIT = 10

if shopping_cart.num_items() >= CART_TOO_BIG_LIMIT:
  Error("Too many items in cart.")
```

CART_TOO_BIG_LIMIT is an ambiguous name

it’s not clear whether you mean “up to” or “up to and including.”

```python
MAX_ITEMS_IN_CART = 10

if shopping_cart.num_items() > MAX_ITEMS_IN_CART:
  Error("Too many items in cart.")
```

---
layout: default
---

# Prefer first and last for Inclusive Ranges

Here is another example where you can’t tell if it’s “up to” or “up to and including”:

```python
print integer_range(start=2, stop=4)
```

Does this print [2, 3] or [2, 3, 4] ?

Refactor with first & last:

```python
print integer_range(first=2, last=4)
```

---
layout: default
---

# Naming The Boolean Variable

When picking a name for a boolean variable or a function that returns a boolean, be sure it’s
clear what true and false really mean

Here’s a dangerous example:

```python
  bool read_password = true;
```

Depending on how you read it, there are two very different interpretations:

- We need to read the password

- The password has already been read

In this case, it’s best to avoid the word `read` and name it `need_password` or
`user_is_authenticated` instead

In general, adding words like `is, has, can, or should` can make booleans more clear.

For example, a function named `SpaceLeft()` sounds like it might return a number. If it were
meant to return a boolean, a better name would be `HasSpaceLeft()`.

---
layout: default
---

# Naming The Boolean Variable

Avoid negated terms in a name

```ts
bool disable_ssl = false;
```

it would be easier to read (and more compact) to say:

```ts
bool use_ssl = true;
```

---
layout: default
---

# Aesthetics - The beautiful codes

![Aesthetics](/images/2.png)

<style type="text/css">
  img {
    max-width: 400px;
    margin: 0 auto;
  }
</style>

---
layout: default
---

# Columns Alignment

```ruby
def count_valid_clients
    counter_valid = 0
    counter_existing = 0
    headers = get_headers
    email_column_idx = headers.find_index(@email) + 1
    emails = @spreadsheet.column(email_column_idx).uniq
    emails = emails.map { |e| e.strip.downcase }
    exist_user_emails = User.where("lower(email) IN (?)", emails).pluck(:email) || []
    exist_client_emails = Client.where("lower(email) IN (?)", emails).pluck(:email) || []
    all_emails_importing = (exist_user_emails + exist_client_emails).uniq
    counter_existing_clients_in_same_clinic = User.where(email: all_emails_importing).where(company_id: company_id).count
    counter_existing_clients_in_other_clinic = User.where(email: all_emails_importing).where.not(company_id: company_id).count
    {
      valid: counter_valid,
      existing: counter_existing_clients_in_same_clinic,
      counter_existing_clients_in_other_clinic: counter_existing_clients_in_other_clinic
    }
end
```

---
layout: default
---

# Columns Alignment

```ruby
def count_valid_clients
    counter_valid    = 0
    counter_existing = 0

    headers          = get_headers
    email_column_idx = headers.find_index(@email) + 1
    emails           = @spreadsheet.column(email_column_idx).uniq
    emails           = emails.map { |e| e.strip.downcase }

    exist_user_emails    = User.where("lower(email) IN (?)", emails).pluck(:email) || []
    exist_client_emails  = Client.where("lower(email) IN (?)", emails).pluck(:email) || []
    all_emails_importing = (exist_user_emails + exist_client_emails).uniq

    counter_existing_clients_in_same_clinic  = User.where(email: all_emails_importing).where(company_id: company_id).count
    counter_existing_clients_in_other_clinic = User.where(email: all_emails_importing).where.not(company_id: company_id).count

    {
      valid: counter_valid,
      existing: counter_existing_clients_in_same_clinic,
      counter_existing_clients_in_other_clinic: counter_existing_clients_in_other_clinic
    }
end
```

---
layout: default
---

# Columns Alignment

```ruby
# ex1:
bool use_ssl             = true;
integer max_item_in_cart = 20;
float black_box_price    = 29.99;
string vn_currency       = "VND";

# ex2:
commands[] = {
  { "timeout",      "NULL",            cmd_spec_time_out },
  { "timestamping", opt.timstamping,   cmd_boolean },
  { "tries",        opt.ntry,          cmd_number_inf },
  { "userproxy",    opt.use_proxy,     cmd_boolean },
  { "useragent",    opt.firefox_agent, cmd_spec_user_agent }
}
```

---
layout: default
---

# Refactor with split the code into the paragraph

```ruby
# Import the user's email contacts, and match them to users in our system.
# Then display a list of those users that he/she isn't already friends with.
def suggest_new_friends(user, email_password):
 friends = user.friends()
 friend_emails = set(f.email for f in friends)
 contacts = import_contacts(user.email, email_password)
 contact_emails = set(c.email for c in contacts)
 non_friend_emails = contact_emails - friend_emails
 suggested_friends = User.objects.select(email__in=non_friend_emails)
 display['user'] = user
 display['friends'] = friends
 display['suggested_friends'] = suggested_friends
 return render("suggested_friends.html", display)
end
```

---
layout: default
---

# Refactor with split the code into the paragraph

```ruby
# Import the user's email contacts, and match them to users in our system.
# Then display a list of those users that he/she isn't already friends with.
def suggest_new_friends(user, email)
  # Get the user's friend email addresses.
  friends = user.friends()
  friend_emails = set(f.email for f in friends)

  # Import all email addresses from this user's email account.
  contacts = import_contacts(user.email, email)
  contact_emails = set(c.email for c in contacts)

  # Find matching users tha they aren't already friends with.
  non_friend_emails = contact_emails - friend_emails
  suggested_friends = User.objects.select(email___in=non_friend_emails)

  # Display these lists on the page.
  display['user'] = user
  display['friends'] = friends
  display['suggested_friends'] = suggested_friends
  return render("suggested_friends.html", display)
end
```

---
layout: default
---

# Consistent style is more important than the “right” style.

```javascript
# ex1:
function onClickSubmit(){
  console.log('test')
}

function onClickCancel()
{
  console.log(`test`)
}

function onClickSave() {
  console.log("test")
}
```

---
layout: default
---

# Consistent style is more important than the “right” style.

```javascript
# ex1:
function onClickSubmit() {
  console.log('test')
}

function onClickCancel() {
  console.log('test')
}

function onClickSave() {
  console.log('test')
}
```

---
layout: default
---

# Making Control Flow Easy to Read

![control flow](/images/3.png)

<style type="text/css">
  img {
    max-width: 600px;
    margin: 0 auto;
  }
</style>

---
layout: default
---

# Making Control Flow Easy to Read

### Make conditional, loops, other changes to control flow as “Natural” as possible

```ruby
if(your_age >= 18) or if(18 >= your_age)

while(your_age <= 18) or while(your_age >= 18)
```

Why first version is more readable ???

Assume we have: A > B:

- Left hand side (A): Many cases It is a value to be checked, It is changed follow by time, It is variable.

- Right hand side (B): It is a stable value, It is a value to be checked with A. Many cases It is a constant.


---
layout: default
---

# The order of IF/ELSE block

```ruby
if (my_chicken_status == "hungry") {
  FeedingWorker.run
} else {
  SleepingWorker.run
}

or

if (my_chicken_status != "hungry") {
  SleepingWorker.run
} else {
  FeedingWorker.run
}
```

The first version is simpler to understand. It near natural says.

---
layout: default
---

# The order of IF/ELSE block

- Prefer dealing with POSSITIVE CASE FIRST, instead of Negative (if (debug) more readable than if(!debug))

- Prefer dealing with SIMPLE CASE FIRST.

- Prefer dealing with the more INTERESTING CASE or SPECIAL CASE or DANGEROUS CASE FIRST.

---
layout: default
---

# The ?: Conditional Expression (a.k.a. “Ternary Operator”)

Instead of minimizing the number of lines, a better metric is to minimize the time
needed for someone to understand it.

```ruby
result = ((sum > 10) ? 10 : ((sum == 10) ? 9 : ((sum - 2 == 1) ? 8 : 7)))

or

result = if (sum > 10) {
  10
} elsif (sum == 10) {
  9
} elsif (sum - 2 == 1) {
  8
} else {
  7
}

```

---
layout: default
---

# Early returning - “guard clauses”

- When you write down a Function, you should deal with the special case first to return early functions.

- Also, you should deal with cases easy first => That make your code clear and clean, avoid complicated, descrease bugs.

- Minimize Nesting IF/ELSE

---
layout: default
---

```ruby
def send_notification
  client = Client.find_by_id(@client_id)

  if client
    ...

    if notify_hsh.values.include?(true)
      ...
    end
  end
end

or

def send_notification
  client = Client.find_by_id(@client_id)
  return unless client
  return unless notify_hsh.values.include?(true)
  ...
```

---
layout: default
---

```javascript
if (user_result == SUCCESS) {
 if (permission_result != SUCCESS) {
   reply.WriteErrors("error reading permissions");
   reply.Done();
   return;
 }
 reply.WriteErrors("");
} else {
 reply.WriteErrors(user_result);
}
reply.Done();

or

if (user_result != SUCCESS) {
 reply.WriteErrors(user_result);
 reply.Done();
 return;
}
if (permission_result != SUCCESS) {
 reply.WriteErrors(permission_result);
 reply.Done();
 return;
}
reply.WriteErrors("");
reply.Done();
```

---
layout: default
---

# Break down GIANT EXPRESSION

### Use Explain variables to capture small expression.

```ruby
if(line.split(":")[0].strip() == root()) {
  doSomeThing(line.split(":")[0].strip());
  updateName(line.split(":")[0].strip());
}

OR

user_name = line.split(":")[0].strip();

if (user_name == root()) {
  doSomeThing(user_name);
  updateName(user_name);
}
```

---
layout: default
---

# Break down GIANT EXPRESSION

### Use summary variables

```javascript
if (request.user.id == document.owner_id){
  <!-- do A -->
}
if (request.user.id == document.owner_id){
  <!-- do B -->
}

OR

var user_owns_document = request.user.id == document.owner_id;
if (user_owns_document) {
  <!-- do A -->
}
if (user_owns_document) {
  <!-- do B -->
}
```

---
layout: default
---

# Break down GIANT EXPRESSION

### Use DE MORGAN LAWS

Use De Morgan Law to resolve complicated giant expression

De Morgan Law says:

```ts
not (A or B or C) <=> (not A) and (not B) and (not C)

not (A and B and C) <=> (not A) or (not B) or (not C)
```

Example:

```ts
if (!(file_exists && !is_protected)) Error("Sorry, could not read file.");

OR

if (!file_exists || is_protected) Error("Sorry, could not read file.");
```

---
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
---

# Thank you for attending!

Code should be easy to understand from now!

![Sonny and Mariel high fiving.](https://content.codecademy.com/courses/learn-cpp/community-challenge/highfive.gif)

<div class="abs-br m-6 flex gap-2">
  NUS Technology, 16 Aug 2023
</div>

<style type="text/css">
  img {
    margin: 0 auto;
  }
</style>

