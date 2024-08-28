+++
author = "Yaheya Quazi"
title = "Hugo Stock Summary ShortCode Module"
date = "2022-06-29"
description = "Create a Stock Summary ShortCode Display from JSON"
tags = [
"technology",
"hugo",
"shortcode"
]
+++
If you are a regular visitor of my site, you may have noticed, I have started posting daily closing bell stock market [summary](../closing-bell-06282022). These pages proceeds with a Market index summary from that day. Let's discuss how I put it together.

**What is a Hugo ShortCode?**

Shortcodes are simple snippets inside your content files calling built-in or custom templates.

**Parts of the Stock Summary ShortCode**

1. A JSON Payload for each day to summarize the data.
2. ShortCode HTML
3. Insertion of the ShortCode into any Mark Down file.

Below is a sample JSON Payload that the Short Code uses -

```
{
"Index1": {
"name": "Dow",
"value": "30946.99",
"change": "-491.27",
"percentage": "-1.56"
},
"Index2": {
"name": "S&P 500",
"value": "3821.55",
"change": "-78.56",
"percentage": "-2.01"
},
"Index3": {
"name": "Nasdaq",
"value": "11181.54",
"change": "-343.01",
"percentage": "-2.98"
},
"Index4": {
"name": "Russel 2K",
"value": "1738.84",
"change": "-32.90",
"percentage": "-1.86"
},
"Index5": {
"name": "VIX",
"value": "28.36",
"change": "1.41",
"percentage": "5.23"
}

}
```

Copy the above payload and call it stocks06282022.json. Place this file under your **Data** folder in Hugo project.


Create a file called stocks.html and place the following html in it. Save the file under your
**layout\shortcodes** folder


```
<h3>Market Summary</h3>
<table class="table table-dark">
    <thead>
         <tr>
        <th scope="col">Index</th>
        <th scope="col">Value</th>
        <th scope="col">Change</th>
        <th scope="col">Percentage</th>
      </tr>
    </thead>
    <tbody>
      {{ $filename := .Get "file" }}
      {{ range index .Site.Data $filename }}
      <tr>
        <td>{{.name}}</td>
        <td>{{lang.FormatNumberCustom 2 .value}}</td>
        <td>{{lang.FormatNumberCustom 2 .change}}</td>
        <td>{{lang.FormatNumberCustom 2 .percentage}}</td>
      </tr>
      {{end}}
     </tbody>
  </table>
```

**Note** I am using bootstrap 4.1 to stylize my table, you can do the same or style it the way you see fit. 

Ok we are now ready to use the ShortCode! Add the following markup where ever you want to display the stock market summary based on your JSON payload. 

"{ { < stocks file="stocks06282022" >}  }"

**Note** the parameter the markdown file is passing to the ShortCode module. It is the file name. The trick is every day, you update your JSON file to a new file (call it whatever makes sense to you) save it in the Data folder, and then you get your summary displayed!

**Stock Summary** JSON payload can be accessed by this service. I am using it in my project. 

http://api.marketstack.com/v1/exchanges?access_key=your-key&limit=3&offset=0

I hope this post was useful!

**Repository**

My entire site is available to [public](https://github.com/yaheya/pulse).
