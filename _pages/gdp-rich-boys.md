---
permalink: /gdp-rich-boys/
title: "No countries for rich men"
author_profile: false
---

<!DOCTYPE html>
<head>
<meta charset="utf-8">
<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://d3js.org/d3-scale-chromatic.v1.min.js"></script>
<script src="https://d3js.org/d3-geo-projection.v2.min.js"></script>
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>

</head>
<body>
<div>
  <h1>These people have more money than these countries</h1>

  <svg id="map" height="450" width="700" style="margin: 0 auto"></svg>

  <div>
    <br>
    <p>
      Thought experiment: if no country could exist if it was poorer than these people how many countries would there be left?
    </p>
    <p>
      <em>Countries are coloured according to their GDP for 2019 (missing values are grey).</em>
    </p>

    <div class="dropdown">
      <button class="btn btn-primary dropdown-toggle" id="rich_people_text" type="button" data-toggle="dropdown">Pick a rich boy
      <span class="caret"></span></button>
      <ul class="dropdown-menu" role="menu" aria-labelledby="menu1" id="rich_people_menu" >
        <li role="presentation"  value="ElonMusk"><a role="menuitem" tabindex="-1" href="#">Elon Musk (197 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Jeff Bezos (182 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Bill Gates (132 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Bernard Arnault (109 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Mark Zuckerberg (95.6 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Warren Buffet (88.3 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Zhong Shanshan (84.7 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Larry Page (81.7 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Sergey Brin (79.1 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">Larry Ellison (79.1 billion US$)</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#" style="font-weight:bold">Reset</a></li>
      </ul>
    </div>

  </div>
</div>

<script>

// The svg
var svg = d3.select("svg"),
  width = +svg.attr("width"),
  height = +svg.attr("height");

// Map and projection
var path = d3.geoPath();
var projection = d3.geoMercator()
  .scale(100)
  .center([0,20])
  .translate([width / 2, height / 2]);

var richPeople = {
  ElonMusk: 197*1e9,
  JeffBezos: 182*1e9,
  BillGates: 132*1e9,
  BernardArnault: 109*1e9,
  MarkZuckerberg: 95.6*1e9,
  WarrenBuffet: 88.3*1e9,
  ZhongShanshan: 84.7*1e9,
  LarryPage: 81.7*1e9,
  SergeyBrin: 79.1*1e9,
  LarryEllison: 79.1*1e9,
  None: 0
};

// Data and color scale
var data = d3.map();
var colorScale = d3.scaleThreshold()
  .domain([
    47271463.3298575,2132236303.53626,8834579231.28547,16130983819.3426,
    34527810956.2191,66892217111.9715,208683162897.277,428807705025.877,
    1402348904195.07,5142812532961.13,87798525859220.9])
  .range(["#F5F5DC","#EADDC6","#E0C4B0","#D5AC9A",
          "#CB9384","#C07B6E","#B56258","#AB4A42",
          "#A0312C","#961816","#8B0000"]);

  // div for fooltip
  var div = d3.select("body").append("div")
      .attr("class", "tooltip")
      .style("opacity", 0)
      .style("position","absolute")
      .style("background-color","white")
      .style("padding","10px")
      .style("border-radius", "5px")
      .style("font-family","helvetica");

// Load external data and boot
function setup_map() {
  d3.queue()
    .defer(d3.json, "https://raw.githubusercontent.com/holtzy/D3-graph-gallery/master/DATA/world.geojson")
    .defer(d3.csv, "https://raw.githubusercontent.com/votarandreventura/datasets/main/gdp_countries_2019.csv",
      function(d) {data.set(d.code, +d.value); })
    .await(ready);
}

d3.select("body").
style("font-family","helvetica").
style("width","50%").
style("margin","0 auto")


function ready(error, topo) {
  // Draw the map
  svg.append("g")
    .selectAll("path")
    .data(topo.features)
    .enter()
    .append("path")
    // draw each country
    .attr("d", d3.geoPath().projection(projection))
    // set the color of each country
    .attr("fill", function(d) {
      d.total = data.get(d.id) || 0;
      if (d.total > 0) {
        var col = colorScale(d.total);
        if (d.total < th) {var col = "#FFFFFF"}
        } else {
          var col = "#808080"}
      return col;
    })
    .on('mouseover', function(d) {
      if (d.total == 0) {
        var country = d.properties.name;
        var value = "No information available"
      } else {
        var country = d.properties.name;
        var value = (d.total / 1e9).toFixed(2) + " billion US$"
      }
      div
      .style("visibility","visible")
      .transition()
      .duration(100)
      .style('opacity',0.9);
      div.html(country + "<br>" + value)
      .style("left", (d3.event.pageX) + "px")
      .style("top", (d3.event.pageY - 28) + "px");
    })
    .on("mouseout", function(d) {
      div.transition()
      .duration(250)
      .style("opacity", 0)
      div
      .transition()
      .delay(250)
      .style("visibility","hidden");
    })
    .on("mousemove",function() {
      return div
      .style("left",(d3.event.pageX + 20) + "px")
      .style("top",(d3.event.pageY + 20) + "px")})
    }

var th = 0;
setup_map();
$(document).ready(function(){
  $(".dropdown-toggle").dropdown();
});

$("#rich_people_menu li a").on('click', function(e) {
  e.preventDefault(); // cancel the link behaviour
  var selText = $(this).text();
  if (selText == "Reset") {
    selText = "Pick a rich boy";
    th = 0;
  } else {
    name = selText.split('(')[0].replace(' ','').replace(' ','');
    th = richPeople[name]
    console.log(th,name)
  }
  $("#rich_people_text").text(selText);
  setup_map();
});
</script>
</body>
