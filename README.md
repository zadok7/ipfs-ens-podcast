# ipfs-ens-podcast

This is a guide for setting up a podcast on IPFS with an ENS name. IPFS (InterPlanetary File System) is a distributed system for storing and accessing files, websites, applications, and data. ENS (Ethereum Name Service) is a decentralized domain name system built on the Ethereum blockchain. Together, they provide a resilient and decentralized infrastructure for hosting a podcast. 

To follow this guide, you will need an ENS name to point to your IPFS content. If you haven't already registered one, you can do so at [app.ens.domains](https://app.ens.domains).

The guide assumes you are using a directory structure for organizing your podcast files. The podcast player reads the metadata from the podcast files stored in the IPFS network, and uses this data to populate the player with episodes. The podcast files and directories follow a specific naming and structural convention to enable the player to automatically update the podcast content just by adding new files to IPFS.

(Note: If your ENS name points to an IPNS location, you do not need to change the meta tags each time you update your content. However, if you are not using an ENS name with the [limo resolver](https://eth.limo), you will need to update the `directoryHash` each time you add new content to the `podcast_content` folder as the IPFS hash of this directory will change.)

## Directory Structure

- **/mypodcast** - The root directory of the podcast.
  - **index.html** - The main HTML file that renders your podcast audio player webpage.
  - **style.css** - The Cascading Style Sheet (CSS) file that contains style information for your podcast audio player webpage.
  - **/generator** - Contains tools or scripts that generate or manage podcast content.
    - **index.html** - A utility page for handling the podcast content generation.
  - **/podcast_content** - Contains all the data related to your podcast episodes. The IPFS hash of this directory is used in the `directoryHash` meta tag of index.html.
    - **podcast.png** - The main image file for your podcast, used as a logo or banner.
    - **podcast.txt** - A general description or information file about your podcast.
    - **/episode_descriptions** - Contains text files that provide descriptions or summaries for individual podcast episodes.
      - **episode1.txt, episode2.txt, episode3.txt** - Description files for respective podcast episodes.
    - **/episode_images** - Contains images related to individual podcast episodes.
      - **episode1.png, episode2.png, episode3.png** - Image files related to respective podcast episodes.
    - **/episodes** - Contains the audio files of the podcast episodes.
      - **episode1.mp3, episode2.mp3, episode3.mp3** - The actual audio files of your podcast episodes.
- **/podcast.rss** - This can be outside of the /mypodcast folder. The file can be generated with /mypodcast/generator/index.html.

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


