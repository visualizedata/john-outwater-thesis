<template>
    <div id="drop" align=center></div>
</template>


<script>
// import radialData from "~/assets/radialBerry.json"
import * as d3 from 'd3';
export default {
  name: 'bar2',
  data() {
      return{
          berryData:[]
	};
	
  },


  mounted() {

          let _this = this

     d3.csv("./berry.csv").then( function(data, error){
        _this.berryData=data
        _this.drawBars();
     })
      console.log("mounted");
      

  },

 
 created(){



    //  console.log(this.berryData);
 
 },

 

 methods: {

drawBars(){
    console.log("drawBars")
    let data=this.berryData
    console.log(this.berryData);

    var margin = {top: 80, right: 180, bottom: 80, left: 180},
        width = 960 - margin.left - margin.right,
        height = 500 - margin.top - margin.bottom;

    var svg = d3.select("body").append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

	var elements = Object.keys(data[0])
		.filter(function(d){
			return (d != "sample_name2");
		});
	var selection = elements[0];

	var y = d3.scaleLinear()
			.domain([d3.min(data, function(d){return +d[selection];})-(0.1), d3.max(data, function(d){return +d[selection];})])
			.range([height, 0]);

	var x = d3.scaleBand()
			.domain(data.map(function(d){ return d.sample_name2;}))
			.range([0, width]);


	var xAxis = d3.axisBottom()
		.scale(x);

	var yAxis = d3.axisLeft()
		.scale(y);

	svg.append("g")
    	.attr("class", "x axis")
    	.attr("transform", "translate(0," + height + ")")
    	.call(xAxis)
    	.selectAll("text")
    	.style("font-size", "8px")
      	.style("text-anchor", "end")
      	.attr("dx", "-.8em")
      	.attr("dy", "-.55em")
      	.attr("transform", "rotate(-90)" );


 	svg.append("g")
    	.attr("class", "y axis")
    	.call(yAxis);

	svg.selectAll("rectangle")
		.data(data)
		.enter()
		.append("rect")
		.attr("class","rectangle")
		.attr("width", width/data.length)
		.attr("height", function(d){
			return height - y(+d[selection]);
		})
		.attr("x", function(d, i){
			return (width / data.length) * i ;
		})
		.attr("y", function(d){
			return y(+d[selection]);
		})
		.append("title")
		.text(function(d){
			return d.sample_name2 + " : " + d[selection];
		});

	var selector = d3.select("#drop")
    	.append("select")
    	.attr("id","dropdown")
    	.on("change", function(d){
        	selection = document.getElementById("dropdown");

        	y.domain([d3.min(data, function(d){return +d[selection.value];})-(0.1), d3.max(data, function(d){return +d[selection.value];})]);

        	yAxis.scale(y);

        	d3.selectAll(".rectangle")
           		.transition()
	            .attr("height", function(d){
					return height - y(+d[selection.value]);
				})
				.attr("x", function(d, i){
					return (width / data.length) * i ;
				})
				.attr("y", function(d){
					return y(+d[selection.value]);
				})
           		.ease("linear")
           		.select("title")
           		.text(function(d){
           			return d.sample_name2 + " : " + d[selection.value];
           		});
      
           	d3.selectAll("g.y.axis")
           		.transition()
           		.call(yAxis);

         });

    selector.selectAll("option")
      .data(elements)
      .enter().append("option")
      .attr("value", function(d){
        return d;
      })
      .text(function(d){
        return d;
      })
  }
}

}
</script>
