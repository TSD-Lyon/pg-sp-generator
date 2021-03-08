/**
* (c) 2008 Development Lab, Steve L. Nyemba nyemba@gmail.com
* Jx Framework 0.7
*
* This file contains basic reusable javascript namespace/packages and some ajax support.
* The namepace/package encapsulates functionalities for:
* 1. Reusable Elements (jx):
*		a. ajax: is a package that will return an XMLHttpRequest Object (compatible with IE 6x+; Mozilla/Firefox; Safari)
*		b. edoc: is a document handler, provides the means to embed and SGML derived document into a DOM element
*		c. xmlparser: is an XML parser intended to be a generic XML parser that will store xml into an array of associative arrays
*		d. utils	: encapsulates a set of reusable functions validation; value extraction
* Things To know:
*	- I am implementing design by contract for any client code to be able to enforce the contract and thus write adequate unit tests.
*	- jsunit Javascript Unit Testing Framework (http://www.voomr.com/jsunit and or http://www.voomr.com/jsunitframework)
*
* References:
* 	- Desgin by contract 	http://www.eiffel.com
* 	- JSUnit 2.2		http://voomr.com/jsunit or http://www.voomr.com/jsunitframework
* What's new:
* 	jx.utils:
* 		isValid		designed to validate an input field given it's id
* 		setXY		this method sets X,Y coordinates depending on the reference frame i.e absolute or relative
* 	jx.edoc
* 		set		now supports INPUT in addition to DIV and SPAN
* Quote:
*	A mission impossible is the framework in which great feats of engineering are expressed, Steve L. Nyemba
*/

var jx={
	/**
	 * This method is intended to be used for testing purposes to see if the framework is alive and running
	 */
	copyRight:function(){
		return ("(c) 2008 The Development Lab, JxFramework 0.7");
	},
	/**
	* This namespace encapsulates reusable javascript operations on document object models (DOM)
	*/
	utils:{
		/**
		 * runs basic validation rules against a given DOM element. Consider the pattern of xml to be used is
		 * 	<domain id="a group">
		 * 		<entry id="min-length" value="avalue"/>
		 * 		<entry id="max-length"	value="aValue"/>
		 * 		<entry id="regex"	value="aRegexExpression"/>
		 * 	</domain>
		 * Things to know:
		 * It is recommended that the document be parsed with jx.xmlparser.parse xml function
		 * if no entries are mandatory the function will return true as we assume no tests failed implying;
		 */
		isValid:function(map,id){
			var p = false;	//-- evaluates minimum length ;
			var q = false;	//-- evaluates maximum length;
			var r = false;	//-- evaluates regular expression
			var dom = document.getElementById(id) ;

			if(map['map']['min-length'] != null){
				p = (dom.value != undefined)?(map['map']['min-length'] >= dom.value.length):(map['map']['min-length'] >= dom.innerHTML.length) ;
			}else{
				p = true ;
			}

			if(map['map']['max-length'] != null){
				q = (dom.value != undefined)?(map['map']['max-length'] <= dom.value.length):(map['map']['max-length'] >= dom.innerHTML.length) ;
			}else{
				q = true ;
			}

			if(map['map']['regex'] != null){
				r = (dom.value != undefined)?(dom.value.matches(map['map']['regex'])):(dom.innerHTML.mathches(map['map']['regex']));
			}else{
				r = true ;
				}
			return p && q && r ;

		},//-- end jx.utils.isValid

		/**
		 * This function will set a dom object to a given position i.e relative or absolute
		 * @x	: x-axis coordinate 	(left)
		 * @y	: y-axis coordinate	(top)
		 * @id	: DOM id
		 * @abs	: true|false by default will be true
		 */
		setXY:function(x,y,id,id,isAbsolute){
			var dom = document.getElementById(id) ;
			dom.style.left 	= x ;
			dom.style.top 	= y;
			if(abs == true | abs == null){
				dom.style.position = 'absolute' ;
			}else{
				dom.style.position = 'relative';
			}
		},//-- end jx.utils.isValid

		/**
		* This function returns the selection that has been made on a radio group given the radiogroup name
		* pre : document.getElementsByName(name) != null
		* post: getRadioGroup(name) != null
		*/
		getRadioGroupValue:function (name){
			var group = document.getElementsByName(name) ;

			if(group != null){
				for (var i=0; i < group.length; i++){
					if(group[i].checked == true){
						return group[i].value ;
					}
				}
			}
			return null;
		},//-- end jx.utils.getRadioGroupValue
		/**
		 * This function return the value(s) of a group of checkboxes given their name and user must specify value separator otherwise comma is used if not specified
		 * pre :
		 * post:
		setXY:function(x,y,id,refId){
		 */
		getCheckBoxValue:function(name,sep){
					var values = new Array();
			var separator = null;
			if(sep == null){
				separator = ',';
			}else{
				separator = sep ;
			}
			var boxes = document.getElementsByName(name) ;
			for(var i= 0 ; i < boxes.length; i++){
				if(boxes[i].checked == true){
					values.push(boxes[i].value) ;
				}
			}
			//
			// returning the values separated with space
			//
			return values.join(separator) ;
		},//-- end jx.utils.getCheckBoxValue

		/**
		 * This function will return the value(s) of a dropdown list given the drop down lists ID
		 * The return values are comma separated if not separator is not specified
		 * pre : document.getElementById(id) != null
		 * post: getDropDownValue(id,sep) != null || (getDropDownValue(id,sep) != null && getDropDownlist(id,sep).match(sep))
		 */
	  	getDropDownValue:function(id,sep){
	  		var dropdown = document.getElementById(id);
			var separator = null;
			if(sep == null){
				separator = ',';
			}else{
				separator = sep ;
			}
		  	var value = new Array() ;
		  	for(var i=0; i < dropdown.length ; i++){
	  			if(dropdown.options[i].selected == true){
			  		value .push(dropdown.options[i].value) ;
			  	}
	  		}
		  	return value.join(separator) ;
		  }//-- end jx.utils.getDropDownValue
	},//-- end jx.utils
	/**
	* This namespace contains functions related to AJAX. Factory for XMLHttpRequest object as well as a build in XML request runner
	*/
	ajax:{
		cache:null,
		xmlhttp:null,
		/**
		* This function returns an XMLHttpRequest Object
		* Pre : navigator.appName == "Netscape" || "Microsoft Internet Explorer"
		* Post: jx.ajax.get() != null
		*/
		get:function(){
			var xmlhttp = null;
			if(window.ActiveXObject){
				xmlhttp = new ActiveXObject("Msxml2.XMLHTTP") ;
			}else{
				//
				// This section is for any browser that supports w3 standards i.e Mozilla; Safari; ...
				//
				xmlhttp = new XMLHttpRequest() ;
			}
			//
			// returning the result
			//
			return xmlhttp ;
		},//-- end of jx.ajax.get()
		/**
		* This function will run an ajax request i.e xmlhttp request given a method i.e POST|GET|PUT
		* The function requires a callback function that will take an xmlDocument as a parameter allowing consuming code to solely manipulate response.
		* TODO:
		* 	Monitor the XML http request that has been sent
		* 	Provide consistent error handling in terms of look & feel and/or make it configurable
		*/
		run:function(p_url,p_callback,p_method){
			var method = p_method;
			if (p_method == null){
				p_method='POST' ;
			}
			jx.ajax.xmlhttp = jx.ajax.get() ;
			//------------------- inner listener
			jx.ajax.xmlhttp.onreadystatechange = function(){
				if(jx.ajax.xmlhttp.readyState == 4){
					p_callback(jx.ajax.xmlhttp) ;
				}
			}
			//------------------- end of listener definition

			jx.ajax.xmlhttp.open(method,p_url,true) ;
			jx.ajax.xmlhttp.send(null) ;

		}//-- end jx.ajax.run(p_url,p_callback) ;

	},//-- end of jx.ajax
	/**
	 * This package contains methods that provide basic management to an embedded document in a DOM element:
	 * set, hide, unhide, clear
	 */
	edoc:{
		/**
		* This method will set the SGML derived document to a DOM element
		* pre : p_SGMLDoc != null && document.getElementById(p_DOMElementId) != null
		* post: p_SGMLDoc == document.getElementById(p_DOMElementId)
		*/
		set:function(p_SGMLDoc,p_DOMElementId){
			var dom = document.getElementById(p_DOMElementId) ;
			var value = p_SGMLDoc ;
			//
			// In case the user had the unbrilliance to swap id and value we can correct this
			//
			if(dom == undefined || dom == null){
				dom = document.getElementById(p_SGMLDoc) ;
				value = p_DOMElementId ;
			}

			if(dom.value != undefined){
				dom.value = value
			}else if(dom.innerHTML != undefined){
				dom.innerHTML = value ;
			}


	    	}, //-- end of jx.edoc.set(p_SGMLDoc,p_DOMElementId)
		/**
		 * This method will hide a DOM element given its ID
		 * pre : document.getElementById(p_DOMElementId) != null && document.getElementById(p_DOMElementId).style.display != 'none'
		 * post: document.getElementById(p_DOMElementId).style.display == 'none'.
		 */
		hide:function(p_DOMElementId){
			     document.getElementById(p_DOMElementId).style.display= 'none'
		     }, //-- end of jx.edoc.hide(p_DOMElementId)
		/**
		* This method will unhide a DOM element given its ID
		* pre : document.getElementById(p_DOMElementId) != null && document.getElementById(p_DOMElementId).style.display == 'none'
		* post: document.getElementById(p_DOMElementId).style.display != 'none'
		*/
		show:function(p_DOMElementId){
			       document.getElementById(p_DOMElementId).style.display = 'block' ;
		     }, //-- end of jx.edoc.hide(p_DOMElementId)
		/**
		 * This method clears the content of a DOM element given its ID
		 * pre :document.getElementById(p_DOMElementId).innerHTML != ""
		 * post:document.getElementById(p_DOMElementId).innerHTML == null
		 */
		clear:function(p_DOMElementId){
			      document.getElementById(p_DOMElementId).innerHTML = "";
		      }//-- end of jx.edoc.clear(p_DOMElement)
	     },//-- end of jx.edoc
	/**
	 * This package contains method to generically parse an XML document
	 * For this parser to be effective, the XML document must NOT refer to other XML elements in the document.
	 * This is to insure simplicity of XML documents and understandability of the parser which will lead to its ease of maintenance.
	 */
	xmlparser:{
		/**
		 * The accepted xml format is as simple as follows:
		 *	<domain id="">
				<entry id="" value=""/>
			</domain>
		 * The idea behind this simplistic representation of an XML data is that it's an asbraction of any given entity
		 */
		parse:function(p_XMLDocument){
			//
			//
			var domainTag 	= 'domain';
			var entryTag	= 'entry';
			var nodes = p_XMLDocument.getElementsByTagName(domainTag);

			var entry;
			var entryId;
			var entryValue;
			var entries;
			var groupName;
			var groupNames = new Array() ;
			var map = new Array();
			for(var i=0; i < nodes.length; i++){

				//
				// the expected xml in the domain tag will ONLY have one attribute or sub tag "id" that uniquely identifies this logical group
				//
				groupName = nodes[i].attributes[0].value ;

				//
				// Because of the implementation of the eef ajax writer the index '2' corresponds to the actual begining of an entry tag perse
				// This index is also based on the data structure that implements our logical grouping.
				// If an attribute is added or removed please consider revising the index
				//
				entry = new Array();
				entries = new Array();
//					alert(nodes[i].childNodes[1].attributes[1].value)

				for(var j=0; j < nodes[i].childNodes.length; j++){
					//
					// now we are extracting the entries ... we know the entries are id,value pairs
					// That is why we use indexes 0 & 1
					//
					if(nodes[i].childNodes[j].attributes != null){
						entryId 	= nodes[i].childNodes[j].attributes[0].value 	;
						entryValue 	= nodes[i].childNodes[j].attributes[1].value ;
						entry[entryId]	= entryValue ;
						entries.push(entryId) ;
					}



				}
				map[groupName] = new Array() ;

				//
				// At this point we have finished constructing a logical group and should therefore store it into the map
				// The key being the GroupName
				map[groupName] = new Array();
				groupNames.push(groupName) ;

				map[groupName]['map'] 	= entry;
				map[groupName]['ids']	= entries;
				 ;
			}
			map['groupNames'] = groupNames ;
			return map;
		} //-- end of jx.xmlparser.parse(p_XMLDoc)
	  }
}

