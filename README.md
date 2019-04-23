# How To Use

Start from `rails new`

```
rails new kamiliff_demo
bundle add kamiliff
```

## Add LIFF endpoint

Login to line developers, and create 3 LIFF for 3 different size.

- for compact
  - name: Compact
  - size: Compact
  - Endpoint URL: https://yourwebsite/liff_entry

- for tall
  - name: Tall
  - size: Tall
  - Endpoint URL: https://yourwebsite/liff_entry

- for full
  - name: Full
  - size: Full
  - Endpoint URL: https://yourwebsite/liff_entry

Since the compact size is default. You could only create the compact one.

## Set environment variables

Install dotenv-rails gem

```
bundle add dotenv-rails
```

Create a file `.env` with the following content:

```
LIFF_COMPACT=line://app/for_compact_liff_id
LIFF_TALL=line://app/for_tall_liff_id
LIFF_FULL=line://app/for_full_liff_id
```

You could choose another setting method.

## Generate simple todo resource

Create todo resource

```
rails g scaffold todo name desc
rails db:migrate
```

## Create LIFF view

Create liff view for new action at `app/views/todos/new.liff.erb`.

```
<%= render "todos/form.html", todo: @todo %>

<script>
document.title = "new todo";

window.addEventListener("liff_submit", function(event){
  var json = JSON.stringify(event.detail.data);
  var url = event.detail.url;
  var method = event.detail.method;
  var request_text = method + " " + url + "\n" + json;
  liff_send_text_message(request_text);
});
</script>
```

The javascript listen to submit button click, and build the message from from data, and send to current LINE chatroom, and then close the LIFF webview.

You could modify those javascript to change the format.

## Test

Add following content into `app/views/todos/index.html.erb`.

```
<%= liff_path(path: new_todo_path) %>
```

Copy this url and paste to any LINE chatroom.

Click this url in LINE app.

The correct usage is put this url into Rich Manu or Flex Message or Template Message with url action.