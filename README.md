# Facebook Page Post Scraper

![](/examples/fb_scraper_data.png)

A tool for gathering *all* the posts and comments of a Facebook Page (or Open Facebook Group) and related metadata, including post message, post links, and counts of each reaction on the post. All this data is exported as a CSV, able to be imported into any data analysis program like Excel.

The purpose of the script is to gather Facebook data for semantic analysis, which is greatly helped by the presence of high-quality Reaction data. Here's quick examples of a potential Facebook Reaction data visualization using data from [CNN's Facebook page](https://www.facebook.com/cnn/):

![](/examples/reaction-example-2.png)

## Usage

### Scrape Posts from Public Page

The Page data scraper is implemented as a Python 2.7 script in `get_fb_posts_fb_page.py` (and for Python 3.5, `py3.5_get_fb_posts_fb_page.py`); fill in the App ID and App Secret of a Facebook app you control (I strongly recommend creating an app just for this purpose) and the Page ID of the Facebook Page you want to scrape at the beginning of the file. Then run the script by `cd` into the directory containing the script, then running `python get_fb_posts_fb_page.py`.

Example CSVs for CNN, NYTimes, and BuzzFeed data are not included in this repository due to size, but you can download [CNN data here](https://dl.dropboxusercontent.com/u/2017402/cnn_facebook_statuses.csv.zip) [2.7MB ZIP], [NYTimes data here](https://dl.dropboxusercontent.com/u/2017402/nytimes_facebook_statuses.csv.zip) [4.9MB ZIP], and [BuzzFeed data here](https://dl.dropboxusercontent.com/u/2017402/buzzfeed_facebook_statuses.csv.zip) [2.1MB ZIP].

### Scrape Posts from Multiple Public Pages

Multiple pages can be scraped at once with the Python 2.7 script `get_fb_posts_fb_page_list.py`. You must fill in the App ID and App Secret in the same way as for the single page script. You must provide the script an input file, e.g. `list.txt`, where each line is a Page ID of a Facebook Page you want to scrape. You then start the scraper with:

    python get_fb_posts_fb_page_list --file list.txt --maxtokens 40

Each page will be saved in JSON format in the directory you run the script. Each post is a JSON object inside of a JSON array for the page. the --maxtokens switch is optional, but will default to not scraping posts that contain more than 40 tokens.

### Scrape Posts from Open Group

To get data from an Open Group, use the `get_fb_posts_fb_group.py` script with the App ID and App Secret filled in the same way. However, the `group_id` is a *numeric ID*. For groups without a custom username, the ID will be in the address bar; for groups with custom usernames, to get the ID, do a View Source on the Group Page, search for "entity_id", and use the number to the right of that field. For example, the `group_id` of [Hackathon Hackers](https://www.facebook.com/groups/hackathonhackers/) is 759985267390294.

![](/examples/entity.png)

You can download example data for [Hackathon Hackers here](https://dl.dropboxusercontent.com/u/2017402/759985267390294_facebook_statuses.csv.zip) [4.7MB ZIP]

### Scrape Comments From Page/Group Posts

To scrape all the user comments from the posts, create a CSV using either of the above scripts, then run the `get_fb_comments_from_fb.py` script, specifying the Page/Group as the `file_id`. The output includes the original `status_id` where the comment is located so you can map the comment to the original Post with a `JOIN` or `VLOOKUP`, and also a `parent_id` if the comment is a reply to another comment.

Keep in mind that large pages such as CNN have *millions* of comments, so be careful! (scraping throughput is approximately 87k comments/hour)

## Privacy

This scraper can only scrape public Facebook data which is available to anyone, even those who are not logged into Facebook. No personally-identifiable data is collected in the Page variant; the Group variant does collect the name of the author of the post, but that data is also public to non-logged-in users. Additionally, the script only uses officially-documented Facebook API endpoints without circumventing any rate-limits.

Note that this script, and any variant of this script, *cannot* be used to scrape data from user profiles. (and the Facebook API specifically disallows this use case!)

## Maintainer

* Max Woolf ([@minimaxir](http://minimaxir.com))

For more information on how the script was originally created, and some tips on how to create similar scrapers yourself, see my blog post [How to Scrape Data From Facebook Page Posts for Statistical Analysis](http://minimaxir.com/2015/07/facebook-scraper/).

## Credits

[Peeter Tintis](https://github.com/Digitaalhumanitaaria), whose [fork](https://github.com/Digitaalhumanitaaria/facebook-page-post-scraper/blob/master/get_fb_posts_fb_page.py) of this repo implements code for finding separate reaction counts per [this Stack Overflow answer](http://stackoverflow.com/a/37239851).

[Marco Goldin](https://github.com/marcogoldin) for the Python 3.5 fork.

## License

MIT

If you do find this script useful, a link back to this repository would be appreciated. Thanks!
