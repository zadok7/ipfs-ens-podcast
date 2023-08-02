# ipfs-ens-podcast

This is a guide for setting up a podcast on IPFS with an ENS name, or without one just using public IPFS gateways. IPFS (InterPlanetary File System) is a distributed system for storing and accessing files, websites, applications, and data. ENS (Ethereum Name Service) is a decentralized naming system built on the Ethereum blockchain. Together, they provide a resilient and decentralized infrastructure for hosting a podcast. 

To follow this guide, it's best to have ENS name to point to your IPFS content. If you haven't already registered one, you can do so at [app.ens.domains](https://app.ens.domains).

The guide assumes you are using a directory structure for organizing your podcast files. The podcast player reads the metadata from the podcast files stored in the IPFS network, and uses this data to populate the player with episodes. The podcast files and directories follow a specific naming and structural convention to enable the player to automatically update the podcast content just by adding new files to IPFS.

(Note: If your ENS name has a `contenthash` pointed to an IPNS location(or IPFS), you do not need to change the meta tags each time you update your content. If the ENS name points to an IPFS location, ie. the hash of `/podcast_content`, you would need to update the ENS name's records each time there are changes to the podcast which would cost gas.)

For a primer on creating decentralized websites with IPFS and ENS names, see [ENS Support Docs: Create a Decentralized Website](https://support.ens.domains/articles/7890637-create-a-decentralized-website)

## Demo of Decentralized Podcast Built on IPFS with an ENS name.

Podcast Web Player - [podcast.showmehow.eth](https://podcast.showmehow.eth.limo)

Podcast Subscription RSS Feed (you can add this to Apple Podcast to subscribe) - [feed.podcast.showmehow.eth](https://feed.podcast.showmehow.eth.limo)

Podcast RSS Feed Generator (works with ENS names or public IPFS gateways - [podcast.showmehow.eth/generator](https://podcast.showmehow.eth.limo/generator)

For other experiments and blog posts about ENS and the blockchain see: [showmehow.eth](https://showmehow.eth.limo)

## Directory Structure

- **/mypodcast** - The root directory of the podcast.
  - **index.html** - The main HTML file that renders your podcast audio player webpage.
  - **style.css** - The Cascading Style Sheet (CSS) file that contains style information for your podcast audio player webpage.
  - **/generator** - Contains a tools for generating the RSS feed.
    - **index.html** - The script to generate and download the podcast.rss subscription file.
  - **/podcast_content** - Contains all the data related to your podcast episodes. The IPFS hash of this directory is used in the `directoryHash` meta tag of index.html.
    - **podcast.png** - The main image file for your podcast.
    - **podcast.txt** - The information file about your podcast.
    - **/episode_descriptions** - Contains text files that provide descriptions or summaries for individual podcast episodes.
      - **episode1.txt, episode2.txt, episode3.txt** - Description files for respective podcast episodes.
    - **/episode_images** - Contains images for each podcast episode.
      - **episode1.png, episode2.png, episode3.png** - Image files related to respective podcast episodes.
    - **/episodes** - Contains the audio files of the podcast episodes.
      - **episode1.mp3, episode2.mp3, episode3.mp3** - The actual audio files of your podcast episodes.
- **/podcast.rss** - This can be outside of the `/mypodcast` folder. The file can be generated with `/mypodcast/generator/index.html`.

## Adding a New Episode

Adding a new episode involves creating and adding new files to the relevant directories. As an example, here's how you can add a fourth episode:

1. **Create your audio file**: Prepare the audio for your fourth episode and save it as `episode4.mp3`.

2. **Add your audio file to the /episodes directory**: Move or copy `episode4.mp3` to the `/mypodcast/podcast_content/episodes/` directory.

3. **Create an image for the episode**: Prepare an image related to your fourth episode and save it as `episode4.png`.

4. **Add your image file to the /episode_images directory**: Move or copy `episode4.png` to the `/mypodcast/podcast_content/episode_images/` directory.

5. **Create a description for the episode**: Write a summary or description for your fourth episode and save it as `episode4.txt`.

6. **Add your description file to the /episode_descriptions directory**: Move or copy `episode4.txt` to the `/mypodcast/podcast_content/episode_descriptions/` directory.

Once you've added these files, the script in `index.html` will automatically recognize the new episode, provided the files follow the naming convention (`episodeX.ext`). No changes to the `index.html` are required.

## index.html

This file primarily serves as the main point of user interaction with your podcast. It hosts the podcast player and dynamically loads your podcast content hosted on IPFS. The content is located through information provided in the `<meta>` tags in the `<head>` section of the file.

The advantage of this setup is that you don't need to edit this file every time you add new episodes to your podcast. The script within the file is designed to automatically fetch directory and file information from IPFS, based on the meta variables set. 

As long as you consistently follow the directory structure and naming conventions described above when adding new episodes, they will seamlessly become available to your listeners without any need to adjust `index.html`.

## Meta Variables

Meta variables serve as pointers that help locate your content in the IPFS network or via your ENS name. They are set in the `<head>` section of `index.html`.

Here's a breakdown of what each variable does and how to update them:

- `ipfsGateway`: This sets the URL of your IPFS gateway. If an ENS name is not provided, this gateway will be used to retrieve the content. 
- `directoryHash`: This should be the IPFS CID hash of the `/podcast_content` directory. If `ensName` is not provided below, this hash will be used to locate the content. Note that each time new content is added to the `podcast_content` folder, the IPFS hash for the directory will change, and this meta tag will need to be updated if you are not using an ENS name.
- `feedHash`: This should be the IPFS hash of your podcast feed. If `feedName` is not provided below, this hash will be used to retrieve the feed.
- `ensName`: If you provide an ENS name here, it will be used to retrieve the content instead of the IPFS gateway and directory hash. Make sure your ENS name resolves correctly to your IPFS content. If your ENS name `contenthash` record points to an IPFS using IPNS then you won't need to update the `contenthash` each time. 
- `feedName`: If you provide an ENS name for the feed, it will be used to retrieve the feed instead of the IPFS gateway and feed hash. 

Update these variables with your specific content information to ensure your podcast can be found and accessed correctly.

```markdown
    <meta name="ipfsGateway" content="https://ipfs.io"> <!-- Sets the IPFS gateway URL. If 'ensName' is not provided, this will be used -->
    <meta name="directoryHash" content="QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds"> <!-- Sets the IPFS directory hash. ie. IPFS CID hash of /podcast_content. If 'ensName' is not provided, this will be used to locate the content -->
    <meta name="feedHash" content="QmaR1NKFxqP3kN9SSKdbVj3p8revjh3qDAQLw9dvfGwqdA"> <!-- Sets the IPFS feed hash. If 'feedName' is not provided, this will be used to retrieve the feed -->
    <meta name="ensName" content="podcast.showmehow.eth"> <!-- If set, this ENS name will be used instead of 'ipfsGateway' and 'directoryHash' to retrieve the content -->
    <meta name="feedName" content="feed.podcast.showmehow.eth"> <!-- If set, this ENS name will be used instead of 'ipfsGateway' and 'feedHash' to retrieve the feed -->
```

## How to Use the /generator/index.html

### Using an ENS Name

1. Open the /generator/index.html file in your web browser.
2. Input your ENS name into the "Podcast ENS Name" field.
3. Click the "Download RSS" button to download your Podcast RSS file.
4. Upload podcast.rss to IPFS and point the `contenthash` record of the ENS subname ie. feed.podcast.showmehow.eth to this IPFS hash. If you use IPNS you won't need to update your ENS subname's record each time.

Here's an example of the generated `podcast.rss` file using an ENS name and the `.limo` resolver.

```markdown
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd">
    <channel>
        <title>The SMH Podcast</title>
        <link>https://podcast.showmehow.eth.limo</link>
        <language>en</language>
        <itunes:author>zadok7.eth</itunes:author>
        <description>The SMH podcast is a demo podcast built on IPFS. We have many great episodes and more coming so be sure to subscribe. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</description>
        <itunes:image href="https://podcast.showmehow.eth.limo/podcast_content/podcast.png" />
        <item>
            <title>Episode three title</title>
            <description>This is a description for episode 3.</description>
            <enclosure url="https://podcast.showmehow.eth.limo/podcast_content/episodes/episode3.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://podcast.showmehow.eth.limo/podcast_content/episode_images/episode3.png" />
            <pubDate>Mon, 19 Jun 2023 00:00:00 GMT</pubDate>
        </item>
        <item>
            <title>Episode two title</title>
            <description>This is a description for episode 2.</description>
            <enclosure url="https://podcast.showmehow.eth.limo/podcast_content/episodes/episode2.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://podcast.showmehow.eth.limo/podcast_content/episode_images/episode2.png" />
            <pubDate>Fri, 19 May 2023 00:00:00 GMT</pubDate>
        </item>
        <item>
            <title>Episode one title</title>
            <description>This is a description for episode 1.</description>
            <enclosure url="https://podcast.showmehow.eth.limo/podcast_content/episodes/episode1.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://podcast.showmehow.eth.limo/podcast_content/episode_images/episode1.png" />
            <pubDate>Wed, 19 Apr 2023 00:00:00 GMT</pubDate>
        </item>
    </channel>
</rss>
```

### Using an IPFS Public Gateway

1. Open the /generator/index.html file in your web browser.
2. Click the "Use IPFS instead" link under the "Podcast ENS Name" field.
3. Input your IPFS gateway into the "IPFS Gateway" field that appears.
4. Input your directory hash into the "Directory Hash" field. (this is the IPFS hash of `/podcast_content`. Remember when you add new episodes this hash changes.)
5. Click the "Download RSS" button to download your Podcast RSS file.

When using your own IPFS public gateway, all generated links will point to your gateway instead of the default .limo resolver.

```markdown
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd">
    <channel>
        <title>The SMH Podcast</title>
        <link>https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds</link>
        <language>en</language>
        <itunes:author>zadok7.eth</itunes:author>
        <description>The SMH podcast is a demo podcast built on IPFS. We have many great episodes and more coming so be sure to subscribe. Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</description>
        <itunes:image href="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/podcast.png" />
        <item>
            <title>Episode three title</title>
            <description>This is a description for episode 3.</description>
            <enclosure url="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episodes/episode3.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episode_images/episode3.png" />
            <pubDate>Mon, 19 Jun 2023 00:00:00 GMT</pubDate>
        </item>
        <item>
            <title>Episode two title</title>
            <description>This is a description for episode 2.</description>
            <enclosure url="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episodes/episode2.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episode_images/episode2.png" />
            <pubDate>Fri, 19 May 2023 00:00:00 GMT</pubDate>
        </item>
        <item>
            <title>Episode one title</title>
            <description>This is a description for episode 1.</description>
            <enclosure url="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episodes/episode1.mp3" length="0" type="audio/mpeg"/>
            <itunes:image href="https://ipfs.io/ipfs/QmXrBdvJ9Rbd4jUkL1XpMHvuiTfEGXjwNNSTaNzgpjfvds/podcast_content/episode_images/episode1.png" />
            <pubDate>Wed, 19 Apr 2023 00:00:00 GMT</pubDate>
        </item>
    </channel>
</rss>
```
You can now subscribe to the decentralized podcast using apps like Apple Podcast.

## Further Development

Ideally, the user would not need to worry about running their own version of IPFS Desktop, or uploading episode files in any particular file structure to IPFS. A no-code tool for building decentralized podcasts on IPFS and ENS names could be made to do all this for you.

