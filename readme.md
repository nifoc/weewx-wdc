[![Tests](https://github.com/Daveiano/weewx-wdc/actions/workflows/test.yml/badge.svg)](https://github.com/Daveiano/weewx-wdc/actions/workflows/test.yml)

# Weewx Weather Data Center skin

- [Weewx Weather Data Center skin](#weewx-weather-data-center-skin)
  - [Key Features](#key-features)
  - [Demo](#demo)
  - [Screenshots](#screenshots)
    - [Stat tiles](#stat-tiles)
    - [Diagram Tiles](#diagram-tiles)
    - [Table Tile](#table-tile)
    - [Climatological days](#climatological-days)
    - [Avg Temperature Calendar](#avg-temperature-calendar)
    - [Forecast & Radar](#forecast--radar)
    - [Full Page](#full-page)
  - [Usage](#usage)
    - [Installation](#installation)
    - [Update from 1.x to 2.x](#update-from-1x-to-2x)
    - [Configuration](#configuration)
      - [Extras](#extras)
      - [DisplayOptions](#displayoptions)
        - [[[diagrams]]](#diagrams)
      - [Units](#units)
    - [Performance](#performance)
    - [Support for weewx-forecast](#support-for-weewx-forecast)
      - [Configuration](#configuration-1)
    - [Localization](#localization)
    - [About page (user-generated content)](#about-page-user-generated-content)
  - [Development](#development)
    - [Scripts](#scripts)
      - [`yarn run dev`](#yarn-run-dev)
      - [`yarn run build`](#yarn-run-build)
      - [`yarn run deploy:local`](#yarn-run-deploylocal)
      - [`yarn run serve:local`](#yarn-run-servelocal)
    - [Ideas for further development](#ideas-for-further-development)
  - [Free Software](#free-software)
  - [Credits](#credits)

Inspired by and build with the [Carbon Design System](https://carbondesignsystem.com/). This skin uses the same technologies as [Weather Data Center](https://github.com/Daveiano/weather-data-center), a cross-platform Desktop App to import and analyze weather data, I wrote.

If you need help installing the skin, please have a look at https://github.com/Daveiano/weewx-interceptor-docker, a configured Dockerfile
which I use as a base for my local PI installation.

If you like the look and feel of the skin please consider having a look into the original [Weather Data Center](https://daveiano.github.io/weather-data-center/)

## Key Features

- Clear and beautiful UI thanks to [IBM Carbon](https://carbondesignsystem.com/) and [nivo](https://nivo.rocks/)
- Configurable Statistic Tiles and Diagram tiles
- Combinable diagrams via skin.conf
- Responsive
- Day, week, month, year and all-time pages
- Archive and NOAA Reports
- Almanac
- Translated for DE and EN
- Tabular representation with Carbon Data Tables
- Climatological days
- Calendar charts for rain days and average day temperature
- Support for [weewx-forecast](https://github.com/chaunceygardiner/weewx-forecast)
- User-generated "About page"
- Classic and alternative layout
- Windrose chart

## [Demo](https://www.weewx-hbt.de)

## Screenshots

### Stat tiles

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-stat-tiles.png)

### Diagram Tiles

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-diagram-tiles.png)

### Table Tile

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/wewx-wdc-table-tile.png)

### Climatological days

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-climatological-days.png)

### Avg Temperature Calendar

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-avg-tmp-calendar.png)

### Forecast & Radar

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-radar-forecast.png)

### Full Page

<table>
    <thead>
        <tr>
            <th>Classic</th>
            <th>Alternative</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td valign="top">

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-classic-01.png)</td>

<td valign="top">

![Screenshot](https://public-images-social.s3.eu-west-1.amazonaws.com/weewx-wdc-01.png)</td>

</tr>
</tbody>
</table>

## Usage

### Installation

**Requires weewx >= 4.5**

**Please note:** For installation, please use the generated zip archive from a release, eg. https://github.com/Daveiano/weewx-wdc/releases/download/v2.0.1/weewx-wdc-v2.0.1.zip.
Don't download the repository directly and don't use the GitHub generated zip and tar.gz archives that come alongside the release. Always use the zip archive named **weewx-wdc-vX.X.X.zip**

Background: The files in the src/ directory are the source files (TypeScript, SCSS). When creating a release, these source files get transformed and optimized, and the output location of these transformed files is the location which matches the location in the install.py script. The weewx-wdc-vX.X.X.zip should contain all these transformed files (like service-worker.js), but if you download the current state of the repo, these files are not included and this will throw multiple `FileNotFoundError` errors while installing. For manual building these files, see [Development](#development).

1. [Download](https://github.com/Daveiano/weewx-wdc/releases) the latest version
2. Create a new folder and unzip to that folder
3. Install the extension: wee_extension --install=path/to/weewx-wdc
4. Restart weewx: weewx restart

For help, please have a look at the [official weewx documentation](https://weewx.com/docs/utilities.htm#wee_extension_utility).

### Update from 1.x to 2.x

Please have a look at the guide from the release notes: https://github.com/Daveiano/weewx-wdc/releases/tag/v2.0.0

### Configuration

The default skin.conf looks like this:

```
# configuration file for the weewx-wdc skin
SKIN_NAME = Weather Data Center
SKIN_VERSION = 2.0.1

[Extras]
    # Show a link to the GitHub respository of this skin. Set to False to hide.
    github_link = True

    # This radar image would be available as $Extras.radar_img
    #radar_img = https://www.dwd.de/DWD/wetter/radar/radfilm_sac_akt.gif
    # This URL will be used as the image hyperlink:
    #radar_url =	https://www.dwd.de/DE/leistungen/radarbild_film/radarbild_film.html

    [[forecast_zambretti]]
        enable = True

    [[forecast_table_settings]]
        source = WU
        num_periods = 72
        num_days = 5
        show_legend = 1
        show_hourly = 0
        show_day = 1
        show_date = 1
        show_outlook = 1
        show_temp = 1
        show_dewpoint = 0
        show_humidity = 0
        show_wind = 1
        show_tides = 0
        show_sun = 1
        show_moon = 1
        show_pop = 1
        show_precip = 1
        show_obvis = 0

[DisplayOptions]
    layout = 'alternative'
    climatological_days = rainDays, summerDays, hotDays, desertDays, tropicalNights, stormDays, iceDays, frostDays
    table_tile_observations = outTemp, outHumidity, barometer, windSpeed, windGust, windDir, rain, rainRate, snowDepth, dewpoint, windchill, heatindex, UV, ET, radiation, appTemp, cloudbase, extraTemp1, extraHumid1, extraTemp2, extraHumid2, extraTemp3, extraHumid3, extraTemp4, extraHumid4, extraTemp5, extraHumid5, extraTemp6, extraHumid6, extraTemp7, extraHumid7, extraTemp8, extraHumid8
    stat_tile_observations = outTemp, outHumidity, barometer, windSpeed, windGust, windDir, rain, rainRate, snowDepth, dewpoint, windchill, heatindex, UV, ET, radiation, appTemp, cloudbase, extraTemp1, extraHumid1, extraTemp2, extraHumid2, extraTemp3, extraHumid3, extraTemp4, extraHumid4, extraTemp5, extraHumid5, extraTemp6, extraHumid6, extraTemp7, extraHumid7, extraTemp8, extraHumid8
    diagram_tile_observations = temp_min_max_avg, tempdew, outHumidity, barometer, windchill_heatindex, wind, windDir, windRose, rain, rainRate, snowDepth, UV, ET, radiation, cloudbase, appTemp
    stat_tile_winddir_ordinal = True
    diagram_tile_winddir_ordinal = True
    windRose_colors = "#f3cec9", "#e7a4b6", "#cd7eaf", "#a262a9", "#6f4d96", "#3d3b72"
    [[diagrams]]
        [[[combined_observations]]]
            [[[[temp_min_max_avg]]]]
                label = "Temperature Min/Max/Avg"
                pointSize = 3
                [[[[[obs]]]]]
                    [[[[[[outTemp_min]]]]]]
                        observation = "outTemp"
                        aggregate_type = "min"
                        color = "#0198E1"
                    [[[[[[outTemp_average]]]]]]
                        observation = "outTemp"
                        aggregate_type = "avg"
                        color = "#666666"
                    [[[[[[outTemp_max]]]]]]
                        observation = "outTemp"
                        aggregate_type = "max"
                        color = "#8B0000"
            [[[[tempdew]]]]
                label = 'Temperature / Dewpoint'
                [[[[[obs]]]]]
                    [[[[[[temp]]]]]]
                        observation = "outTemp"
                    [[[[[[dew]]]]]]
                        observation = "dewpoint"

            [[[[windchill_heatindex]]]]
                label = 'Windchill / Heatindex'
                [[[[[obs]]]]]
                    [[[[[[chill]]]]]]
                        observation = "windchill"
                        color = '#0099CC'
                    [[[[[[heat]]]]]]
                        observation = "heatindex"
                        color = '#610000'

            [[[[wind]]]]
                label = 'Wind speed / Gust speed'
                [[[[[obs]]]]]
                    [[[[[[speed]]]]]]
                        observation = "windSpeed"
                    [[[[[[gust]]]]]]
                        observation = "windGust"

        [[[line]]]
            lineWidth = 2
            pointSize = 5
            isInteractive = True
            enablePoints = True
            enableCrosshair = True
            # @see https://github.com/plouc/nivo/blob/master/packages/line/index.d.ts#L144
            curve = "natural"
        [[[bar]]]
            enableLabel = False
            isInteractive = True
        [[[windDir]]]
            curve = "basis"
            lineWidth = 0
        [[[radiation]]]
            curve = "basis"
        [[[UV]]]
            curve = "step"
        [[[rainRate]]]
            curve = "linear"

        [[[heights]]]
            [[[[classic]]]]
                height = "220px"
                height_md = "300px"
                height_lg = "250px"
                height_xlg = "225px"
                height_max = "225px"
            [[[[alternative]]]]
                height = "220px"
                height_md = "325px"
                height_lg = "325px"
                height_xlg = "250px"
                height_max = "250px"

[CheetahGenerator]
    encoding = html_entities
    search_list_extensions = user.weewx_wdc.WdcGeneralUtil, user.weewx_wdc.WdcStatsUtil, user.weewx_wdc.WdcDiagramUtil, user.weewx_wdc.WdcCelestialUtil, user.weewx_wdc.WdcArchiveUtil, user.weewx_wdc.WdcTableUtil

    [[SummaryByMonth]]
        # Reports that summarize "by month"
        [[[NOAA_month]]]
            encoding = normalized_ascii
            template = NOAA/NOAA-%Y-%m.txt.tmpl
            #stale_age = 3600 # Every hour
        [[[summary_month]]]
            template = month-%Y-%m.html.tmpl
            #stale_age = 3600 # Every hour

    [[SummaryByYear]]
        # Reports that summarize "by year"
        [[[NOAA_year]]]
            encoding = normalized_ascii
            template = NOAA/NOAA-%Y.txt.tmpl
            #stale_age = 3600 # Every hour
        [[[summary_year]]]
            template = year-%Y.html.tmpl
            #stale_age = 3600 # Every hour

    # Reports that show statistics "to date", such as day-to-date,
    # week-to-date, month-to-date, etc.
    [[ToDate]]
        [[[day]]]
            template = index.html.tmpl

        [[[week]]]
            template = week.html.tmpl

        [[[month]]]
            template = month.html.tmpl

        [[[year]]]
            template = year.html.tmpl
            #stale_age = 3600 # Every hour

        [[[statistics]]]
            template = statistics.html.tmpl
            stale_age = 43200 # Twice a day

        [[[celestial]]]
            template = celestial.html.tmpl

    # Static pages, add as many as you want.
    [[Static]]
        #[[[about]]]
            #template = about.html.tmpl
            #title = About

[Units]
    [[TimeFormats]]
        # @see https://weewx.com/docs/customizing.htm#Units_TimeFormats
        day        = %X
        week       = %x
        month      = %x
        year       = %x
        rainyear   = %x
        current    = %x %X
        ephem_day  = %X
        ephem_year = %x
        stats      = %x %X

[CopyGenerator]
    copy_once = dist/main.js, dist/main.css, plotly-custom-build.min.js, favicon.ico, manifest.json, icon-192x192.png, icon-256x256.png, icon-384x384.png, icon-512x512.png, service-worker.js, offline.html
    # copy_always =

[Generators]
    generator_list = weewx.cheetahgenerator.CheetahGenerator, weewx.reportengine.CopyGenerator
```

#### Extras

`github_link` Set to `False` to disable.

`radar_img` and `radar_url` Same as in the default Seasons Skin

#### DisplayOptions

`layout` Switch between `classic` or `alternative`. See [Screenshots](#full-page) for comparison. [weewx-hbt.de](https://weewx-hbt.de) uses the alternative layout. The classic layout can be seen here: https://weewx-hbt.de/classic

`climatological_days` E.g. Rain days (days with precipitation) or Summer days (Temp > 25°C). Leave empty to disable. When activated a rain days calendar chart and an average temperature calendar chart are shown alongside.

`table_tile_observations` Defines which observations should be shown in the data table component.
Simply comment this out or leave empty to hide the tables at all pages:
`#table_tile_observations = outTemp, outHumidity, barometer, ...`

`stat_tile_observations` Define which observations should be shown in the stat tiles (at the top of each page). If you enable `windDir` or `windGustDir`, these will not be added as separate tiles (or rows in the classic layout), but be shown along with the corresponding `windSpeed` and `windGust`.

`diagram_tile_observations` Define which observations to show as diagrams. This can include definitions for combined diagrams. There are two different charts available for the wind direction: `windDir` will render a scatter diagram, `windRose` will render a windrose chart.

Combined diagrams (like Temperature and Dew point or Windchill and Heat index) need to be defined in the `[[diagrams]][[combined_observations]]` section like this:

For a combined diagram of Temperature and Dew point:

```
[[[tempdew]]]                           # 1
    label = 'Temperature / Dewpoint'    # 2
    pointSize = 3                       # 3
    curve = "linear"                    # 3
    [[[[obs]]]]                         # 4
        [[[[[outTemp]]]]]
            observation = "outTemp"     # 5
            aggregate_type = "avg"
        [[[[[dewpoint]]]]]
            observation = "dewpoint"    # 5
            aggregate_type = "avg"      # 6
            color = '#610000'           # 7
```

`# 1` Name of the combined diagram, needs to be the same as used in `diagram_tile_observations`.

`# 2` Label of the diagram.

`# 3` Any additional diagram config available (see [[[diagrams]]](#diagrams)).

`# 4` Under the key `obs` specify the observations to combine (The keys here are random and only used internally).

`# 5` Set the observation to show.

`# 6` Optionally, define the aggregate type (min, max, avg, sum)

`# 7` Optionally, define a colour.

`stat_tile_winddir_ordinal` Show ordinals (S, E, SE, etc.) on the wind direction stat tile.

`diagram_tile_winddir_ordinal` Show ordinals legend in wind direction diagram.

`windRose_colors` The skin uses six different scales for the windrose, each with its own colour, by default:

- ![#f3cec9](https://via.placeholder.com/15/f3cec9/f3cec9.png) `#f3cec9` for <= Beaufort 1
- ![#e7a4b6](https://via.placeholder.com/15/e7a4b6/e7a4b6.png) `#e7a4b6` for Beaufort 2
- ![#cd7eaf](https://via.placeholder.com/15/cd7eaf/cd7eaf.png) `#cd7eaf` for Beaufort 3
- ![#a262a9](https://via.placeholder.com/15/a262a9/a262a9.png) `#a262a9` for Beaufort 4
- ![#6f4d96](https://via.placeholder.com/15/6f4d96/6f4d96.png) `#6f4d96` for Beaufort 5
- ![#3d3b72](https://via.placeholder.com/15/3d3b72/3d3b72.png) `#3d3b72` for >= Beaufort 6

When changing the colours, you can also use rgb() values or just "red".

##### [[diagrams]]

Besides the `combined_observations`, you can configure the look of the diagrams in the skin in the `diagrams` section.

For general behaviour changes, use the options under `[[line]]` and `[[bar]]`:

```
[[[line]]]
    lineWidth = 2
    pointSize = 5
    isInteractive = True
    enablePoints = True
    enableCrosshair = True
    curve = "natural"
[[[bar]]]
    enableLabel = False
    isInteractive = True
```

`lineWidth` The line width (line diagrams)

`pointSize` The point size (line diagrams)

`isInteractive` Enable/disable interactive tooltips (line and bar)

`enablePoints` Show points at all (line diagrams)

`enableCrosshair` Show the crosshair on mouse over (line and bar)

`enableLabel` Show the value as label on the bars (bar)

`curve` Curve interpolation. One of basis, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore (line diagram)

You can configure every diagram under the given observation key like:

```
[[diagrams]]
    ...
    [[[windDir]]]
        curve = "basis"
    [[[radiation]]]
        curve = "basis"
        color = "black"
    [[[UV]]]
        curve = "step"
        lineWidth = 1
    [[[rainRate]]]
        curve = "linear"
        enablePoints = False
    [[[windSpeed]]]
        curve = "linear"
    [[[windGust]]]
        curve = "linear"
        isInteractive = False
```

`[[[heights]]]` Configure the height of the diagram tiles seperate for each layout, classic and alternative per screen size.

Breakpoints are:

md: > 672px <br/>
lg > 1056px <br/>
xlg > 1312px <br/>
max > 1920px <br/>

#### Units

Under the `[[TimeFormats]]` key, you can specify the time formats used to format the headings of the pages. Like the heading "27.06.2022 07:55:00" (current = %x %X) on the index page. For more info see the [weewx documentation](https://weewx.com/docs/customizing.htm#Units_TimeFormats). The TimeFormat `stats` is used for the Min/Max date display in the stat tile and table.

### Performance

You should expect long generation times when using this theme (~25s for all templates on a Raspberry Pi 4B with 15 months of data). If you are getting into
trouble because of this you can comment out the `stale_age` options in skin.conf at the templates section, eg. for the years page:

```
[[[year]]]
    template = year.html.tmpl
    stale_age = 3600 # Every hour
```

This will generate the year.html page only once an hour. `stale_age` is in seconds, see https://weewx.com/docs/customizing.htm#CheetahGenerator. You can experiment with the `stale_age` options to find a good balance between being 'up-to-date' and reasonableness.

`statistics.html` stale age is 43200 (twice a day) by default because it's the most performance-expensive template to generate.

### Support for weewx-forecast

First, install the weewx-forecast extension. I am personally not using the original extension https://github.com/matthewwall/weewx-forecast,
but a fork of it: https://github.com/chaunceygardiner/weewx-forecast. The original produced an error for me preventing it to work (https://github.com/matthewwall/weewx-forecast/issues/7).

For help on configuring the extension, see https://github.com/chaunceygardiner/weewx-forecast.

Then, add `user.forecast.ForecastVariables` and `user.weewx_wdc_forecast.WdcForecastUtil` to `[CheetahGenerator].search_list_extensions` in weewx-wdc skin.conf.

It should look like this

```
...
[CheetahGenerator]
    encoding = html_entities
    search_list_extensions = user.weewx_wdc.WdcGeneralUtil, user.weewx_wdc.WdcStatsUtil, user.weewx_wdc.WdcDiagramUtil, user.weewx_wdc.WdcCelestialUtil, user.weewx_wdc.WdcArchiveUtil, user.weewx_wdc.WdcTableUtil, user.weewx_wdc_forecast.WdcForecastUtil, user.forecast.ForecastVariables
...
```

#### Configuration

Located in skin.conf, section `[Extras]`:

`forecast_zambretti` Enable/Disable Zambretti forecast.

`forecast_table_settings` Predefined and tested configuration values for the forecast table template (the descriptions are partly taken from https://github.com/matthewwall/weewx-forecast/blob/master/skins/forecast/forecast_table.inc):

|                   |                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `source=WU`       | **Important:** Please set `source` to your used forecast source (NWS, WU, OWM, UKMO, Aeris, DS).                          |
| `num_periods=72`  | How many forecast periods should be considered? Up to 40 for NWS, up to 240 for WU. More periods take longer to generate. |
| `num_days=5`      | If number of days is specified, then display up to that many days                                                         |
| `show_legend=1`   | Show the legend column?                                                                                                   |
| `show_hourly=0`   | Not yet supported.                                                                                                        |
| `show_day=1`      | Show the day (Mon, Tue, Wed, ...)?                                                                                        |
| `show_date=1`     | show the date (02 Aug, 03 Aug, 04 Aug, ...)?                                                                              |
| `show_outlook=1`  | Show the outlook icon?                                                                                                    |
| `show_temp=1`     | Show the temperature?                                                                                                     |
| `show_dewpoint=0` | Show the dew point?                                                                                                       |
| `show_humidity=0` | Show the humidity?                                                                                                        |
| `show_wind=1`     | Show the wind speed?                                                                                                      |
| `show_tides=0`    | Show the tides?                                                                                                           |
| `show_sun=1`      | Show sunrise and sunset?                                                                                                  |
| `show_moon=1`     | Show moonrise and moonset?                                                                                                |
| `show_pop=1`      | Show the probability of precipitation?                                                                                    |
| `show_precip=1`   | Show the precipitation?                                                                                                   |
| `show_obvis=0`    | Not yet supported.                                                                                                        |

`show_hourly` and `show_obvis` are not yet supported.

### Localization

The skin currently has an English and a german translation, you can change the language in weewx.conf:

```
[[WdcReport]]
    skin = weewx-wdc
    enable = true
    lang = de
```

### About page (user-generated content)

The skin includes an optional [About page](https://www.weewx-hbt.de/about.html). It is disabled by default in the skin.conf and meant
to be customized by the user. One could for example add an "About my station" page. More info on how to enable and
editing the page can be found [here](https://www.weewx-hbt.de/about.html).

## Development

The skin uses the Cheetah templating engine provided by weewx in combination with carbon web components
and a react entry point to render the diagrams written in TypeScript via nivo. Bundling for Typescript and SCSS is done via webpack.

To start developing, you first need to have Node (npm) installed (>= v14).
Then, install yarn (https://yarnpkg.com/getting-started/install).

Install the dependencies: `yarn install`

### Scripts

#### `yarn run dev`

Starts webpack in watch mode

#### `yarn run build`

Builds the assets

#### `yarn run deploy:local`

Only works if weewx is installed via package, see https://weewx.com/docs/setup.htm.
Copies all skin files in the corresponding weewx installation folders, restarts weewx and triggers a new generation of files via wee_reports:

`sudo cp -R ./skins/weewx-wdc /etc/weewx/skins && sudo cp -r ./bin/user/. /usr/share/weewx/user/ && sudo systemctl restart weewx && sudo /usr/share/weewx/wee_reports`

#### `yarn run serve:local`

Only works if weewx is installed via package, see https://weewx.com/docs/setup.htm.
Starts a nginx docker container to serve the generated files.

`docker run -it --rm -d -p 8080:80 --name web -v /var/www/html/weewx:/usr/share/nginx/html nginx`

### Ideas for further development

- Add Temperature, Rain pages like in WDC

## Free Software

This skin uses only free software. You can read more about [Carbon IBM](https://github.com/carbon-design-system/carbon) (licensed under the Apache-2.0 license) here: https://carbondesignsystem.com/contributing/overview/#introduction. [nivo](https://github.com/plouc/nivo) is licensed under the MIT license.

## Credits

Thanks to [ngulden](https://github.com/ngulden) for the [niculskin](https://github.com/ngulden/niculskin) and
[neoground](https://github.com/neoground) for the [neowx-material skin](https://github.com/neoground/neowx-material). Both are amazing skins and gave me a basic understanding of creating a weewx skin.

The config, NOAA Reports and some templating ideas and concepts are based on the original Standard and Seasons
weewx skins by Tom Keffer and the weewx contributors.
