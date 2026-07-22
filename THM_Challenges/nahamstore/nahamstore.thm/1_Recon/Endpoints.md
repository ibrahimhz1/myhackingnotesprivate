---
title: Endpoints
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - endpoints
  - recon
publish: false
draft: false
description: "Discovered endpoints for nahamstore.thm on ports 80 and 8000"
---

# Endpoints

## nahamstore.thm:80

Endpoints discovered:

`GET /register`

`POST /register`
```
register_email=test@test.com&register_password=password
```

`GET /logout`

`GET /product?id=1`

`GET /product?id=1&added=1`

`POST /stockcheck`
```
product_id=1&server=stock.nahamstore.thm
```

`GET /account/orders`

`GET /login?redirect_url=/orders`

`GET /account/addressbook?redirect_url=/basket`

`POST /account/addressbook?redirect_url=/basket`
```
new_address_title=Mr&new_address_fname=name1&new_address_lname=name2&new_address_line1=lsdf&new_address_line2=lksdf&new_address_line3=lksdf&new_address_state=india&new_address_zipcode=20323
```

`GET /account/settings`

`GET /account/addressbook`

`POST /account/addressbook`
```
new_address_title=Mr&new_address_fname=fname&new_address_lname=lname&new_address_line1=passw&new_address_line2=osdf&new_address_line3=ksdf&new_address_state=india&new_address_zipcode=604510234
```

`GET /basket`

`POST /basket`
```
address_id=5
```

`POST /basket`
```
address_id=5&card_no=1234123412341234
```

`GET /returns`

`POST /returns` (multipart form data):
```
order_number=00004
return_reason=1
return_info=wefwef
```

`GET /returns/1?auth=c4ca4238a0b923820dcc509a6f75849b`

`GET /account/orders`

`GET /account/orders/4`

`GET /search-products?q=z`

`POST /pdf-generator`
```
what=order&id=4
```

`GET /search?q=hoodie`

`GET /search-products?q=hoodie`

`GET /product/picture/?file=cbf45788a7c3ff5c2fab3cbe740595d4.jpg`

## nahamstore.thm:8000

Endpoints discovered:

`GET /admin` - redirects to `/admin/login` if not logged in

`GET /admin/login`

`POST /admin/login`
```
username=admin&password=admin
```

`GET /8d1952ba2b3c6dcd76236f090ab8642c`

`GET /admin/8d1952ba2b3c6dcd76236f090ab8642c`

`POST /admin/8d1952ba2b3c6dcd76236f090ab8642c`
```
name=Pre+Opening+Interest+test&code=<full HTML template>
```

`GET /admin?updated=1`
