fireball
========

3.5k databinding / templates / event handlers for firebase, no jquery required

Here's an example app:

		<script src='https://cdn.firebase.com/v0/firebase.js'></script>
		<script src="fireball.js"></script>

		<div class="page" id="home">
			<h1>Cities</h1>
			<ul id="cities"><li class="city nav"><h3 class="name"></h3></li></ul>
			<ul><li><a id="add_city">Add a city</a></li></ul>
		</div>
		<div class="page" id="city">
			<h1 id="city_name"></h1><a id="back">Go back</a>
			<h2>Places here</h2>
			<ul id="places"><li class="place"><h3 class="name"></h3></li></ul>
			<a class="button" href="#" id="add_place">Add a place</a>
		</div>

		<script>
		var page = "home";
		Fireball("https://fireball-example.firebaseio.com/", {

			live_updating:{
				'#cities':    "cities[]", 
				'#places':    "by_city/$city/locations[]",
				'#city_name': "cities/$city/name"
			},

			on_click:{
				'#add_place': function(){
					var name; if (name = prompt('Name:')) Fireball('#places').push({ name: name });
				},
				'#add_city': function(){
					var name; if (name = prompt('Name:')) Fireball('#cities').push({ name: name });
				},
				'.city': function(el){ Fireball.set('$city', el.id); page = 'city'; },
				'.place': function(el){ alert('You clicked ' + el.data.name); },
				'#back': function(){ page = 'home'; }
			},

			show_when:{
				'.page': function(el){ return page == el.id; }
			}

		});
		</script>
