# Main script

	$ '.inner'
	.unslider
		delay: false
		keys: false


## Light switch

Add a click handler to the light switch.

	$ '#lightswitch, .top, .bottom'
	.click (e)->
		e.preventDefault()

		window.clearTimeout mainTimer

		$ '#lightswitch'
		.toggleClass 'on'

		$ '.contain, .inner'
		.toggleClass 'open'

		$ 'body'
		.toggleClass 'active'


	mainTimer = window.setTimeout ->
		$ '#lightswitch'
		.addClass 'notify'

		window.setTimeout ->
			$ '#lightswitch'
			.removeClass 'notify'
		, 2000
	, 5000


## Page links

Add a click handler for each of the page links.

	$ 'nav a'
	.click (e)->
		e.preventDefault()

Get the link that was clicked.

		a = $ this

		if history
			href = a.attr 'href'
			history.pushState {}, '', href

		slidey = $ '.inner'
		unslider = slidey.data 'unslider'
		unslider.move a.data 'index'

		$ 'nav a'
		.removeClass 'active'

		a.addClass 'active'


## Gallery

	$ '.gallery a'
	.colorbox
		close: '<span class="icon-times">&times;</span>'
		current: ''
		height: '100%'
		next: '<span class="icon-angle-right"></span>'
		previous: '<span class="icon-angle-left"></span>'
		rel: 'gal'


## Map

	google.maps.event.addDomListener window, 'load', ->
		styles = [
			stylers: [
				saturation: -100
			,
				lightness: -10
			]
		,
			elementType: 'geometry'
			featureType: 'road'
			stylers: [
				lightness: 100
			]
		]

		mapOptions =
			center: new google.maps.LatLng -42.885187, 147.336267
			streetViewControl: false
			zoom: 16

		contentString = '<div id="content">
			<p>We meet on the last Wednesday of every month from 5.30pm at:</p>
			<address>The Typewriter Factory,
				<br>Level 3, 13-17 Castray Esplanade,
				<br>Hobart, Tasmania,
				<br>Australia
			</address>
		</div>'

		map_el = document.getElementById 'map'
		map = new google.maps.Map map_el, mapOptions

		infowindow = new google.maps.InfoWindow
			content: contentString
			maxWidth: 200

		marker = new google.maps.Marker
			map: map
			position: new google.maps.LatLng -42.886681, 147.336267
			title: 'The Typewriter Factory'

		map.setOptions
			styles: styles

		infowindow.open map, marker
