# jquery-scroll-on-mongoose

A front-end jquery plugin, provide endless scrolling function based on [Jonas's mongoose-paginator module](https://www.npmjs.org/package/mongoose-paginator) on the back-end. Benefit: avoid usage of cursor.skip in mongodb

## Install

This is a FRONT-END jQuery plugin, this is NOT a NodeJS module.

So just copy `./lib/jquery-scroll-on-mongoose.js` to your project

## Usage
```javascript
      $(window).mongooseEndlessScroll({
        itemsToKeep: 15,
        serviceUrl : "/tickets/list.json",
        container : $("#list"),
        intervalFrequency: 200,
        elControlUp : $("#loading-prev"),
        elControlDown : $("#loading-next"),
        htmlEnableScrollUp : "<i class='fa fa-chevron-up'></i> More Newerer Tickets",
        htmlEnableScrollDown :  "<i class='fa fa-chevron-down'></i> More Older Tickets",
        htmlLoading : "<i class='fa fa-spinner fa-spin'></i> Loading...",
        formatItem: function(item) {
          return "<a href=\"/tickets/" + item._id + "\" class=\"list-group-item\" id=\"" + item._id + "\">\n  <div class=\"row\"><div class=\"col-md-1\">" + (genLabelByStatus(item.status)) + " </div>\n  <div class=\"col-md-2\"><small><code>" + item._id + "</code></small></div>\n  <div class=\"col-md-5\">" + item.title + "</div>\n  <div class=\"col-md-1\">" + item.category + "</div>\n  <div class=\"col-md-1 text-right\">" + (genDateTag(item.created_at)) + "</div>\n  <div class=\"col-md-1 text-right\">" + (genDateTag(item.updated_at)) + "</div>\n  <div class=\"col-md-1\">" + item.attempts + "</div></div></a>";
        }
      });
```


## Sample code of server side controller

```coffee

exports.list = (req, res, next)->
  debuglog "list req.query: %j", req.query

  query = Ticket.paginate(req.query || {}, '_id').select('-comments -content')

  if req.query.before?
    query.sort _id : "asc"
  else
    query.sort _id : "desc"

  query.execPagination (err, result)->
    return next err if err?
    result.success = true
    res.json result
  return
```

## License
Copyright (c) 2014 Yi
Licensed under the MIT license.
