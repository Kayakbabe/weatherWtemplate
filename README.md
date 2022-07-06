# weatherWtemplate
MagicMirror modified default weather module to allow use of a custom template


The compliments module is great. I love the remoteFile config variable so that I can add my own custom list of special date messages and other things. The beauty is that a custom file of compliments doesn’t get erased upon updating the mm and is easy to copy to another mirror instance. It is also easy to find to add or alter the compliments.

The weather module uses external template files like current.njk and forcast.njk. It is possible to modify them to make the weather appear as we like. But they are at risk of being overwritten by an update to the mirror.

I think that having a weather module config variable for a custom template would make updating less stressful and easier.

To that end I have modified my local weather module instance as follows.

In the weather module weather.js defaults section I added

	defaults: {
		remoteTemplate: "current.njk",
and I altered the getTemplate function to the following:

	// Select the template depending on the display type.
	getTemplate: function () {
		
		if (this.config.remoteTemplate !="") {return this.config.remoteTemplate}
		 else {
		
			switch (this.config.type.toLowerCase()) {
				case "current":
					return "current.njk";
				case "hourly":
					return "hourly.njk";
				case "daily":
				case "forecast":
					return "forecast.njk";
				//Make the invalid values use the "Loading..." from forecast
				default:
					return "forecast.njk";
			}
	   }
	},
Thus, if I do not create or set a remote template, the module will not fail, it will get the template it should.
But I want to use a custom template becuase I want control to the css tags used in the html so I can more easily customise the weather. I also wanted a different layout. So made copies of current.njk and forecast.njk and put them in the mm config folder. I also renamed them to be very clear they were my custom template.
Then,
in the weather section of my config.js
I added the relative path to my custom template file.
	
	config: {
		remoteTemplate: "../../../../config/kellyforecast.njk",
	
I actually have a custom version of current and forecast templates. I can quickly tweak the templates and not get into the folder structure of the mm default modules.

I would like to contribute this to the mm repository.
