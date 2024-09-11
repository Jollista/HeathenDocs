# Heathen Documentation
This repository hosts the documentation for Heathen. I think it is incredibly useful to have game design documents when making any sort of game, and I figured this would be better than google docs since a wiki format is easier to navigate, read, and write new articles for.
## Attribution
This documentation was built using [Obsidian](https://obsidian.md/), a free notetaking program available on most platforms. It is not required that you use it to edit or create new docs, but it is **highly recommended**.

The documentation webpage was built using [Quartz 4.0](https://github.com/jackyzha0/quartz) which is an open-source project that transforms markdown content into functional static websites. The documentation for Quartz can be found here: https://quartz.jzhao.xyz/.
# Usage
In order to view the most up to date version of the documentation, simply visit [link to webpage goes here].

In order to edit and create new docs, you will need to follow the installation guide below.
# Installation
## Dependencies
- [Node](https://nodejs.org/en) **v20** or higher
- `npm` **v9.3.1** or higher

Once all the dependencies are installed, in your terminal of choice, enter the following commands:
```
//clone the remote repository
git clone https://github.com/Jollista/HeathenDocs.git

//move into the local repository
cd HeathenDocs

//install additional dependencies and set up quartz
npm i
npx quartz create
npm audit fix
```

Finally, if you're using [Obsidian](https://obsidian.md/), open the folder containing the repository as a vault.
## Contribution
### How to Write Good Docs
Watch [this video](https://youtu.be/ZE8v7uVGepM?si=0xRAmQpgEUBtHI2P). If you don't want to, here are some quick bits of advice from it:
1. Docs should be 1000 words or less, otherwise no one's gonna read 'em. If the feature you're documenting requires more than that, it should be broken down into separate documents to better explain the component pieces.
2. Use pictures (see below). They're worth a thousand words, but take way less time to read and can be extremely helpful in capturing a reader's attention as well as illustrating something that's otherwise hard to explain.
3. Be consistent and specific when using terms, and ensure those terms are defined. If everyone's using inconsistent terms to refer to something, it'll get confusing for everyone and turn into a whole mess really quickly. One way to help with specificity is to write in bits of pseudocode into the docs, that way other designers can give you more clear feedback on your design, and it's easier for programmers to implement later.
### WikiLinks
[Obsidian](https://obsidian.md/) and [Quartz 4.0](https://github.com/jackyzha0/quartz) both use a system of WikiLinks denoted by two sets of square brackets \[\[like this]]. This links one page to another by the shortest available path. You should only ever need to use the name of a file to link to it, but Quartz can be a bit finnicky sometimes, so if you have any problems let me know.
### Tags
Tags are a useful way to organize files other than just putting them in folders. Whenever you make a new doc, make sure it has the appropriate tags.
### Pictures
If you're using Obsidian, linking to an image might mess up. As with WikiLinks, you should typically only use the name of the image you're linking to, or else Quartz might break and fail to render the image properly when it's public for everyone to see. So if Obsidian autofills an image link to something like 
> \!\[\[public/0---Assets/image.png]]

replace that stupid link with just 
> \!\[\[image.png]].
### Previewing and Pushing
You can view your local changes by running
> npx quartz build --serve

Once you're ready to commit your changes, run
> npx quartz sync

> [!NOTE]
> `npx quartz sync` pulls from the remote repository then pushes. In order to push successfully to the remote repository you must be added as a collaborator (just ask me in discord and I'll add you).

> [!IWARNING] Merge Conflicts
> Be careful of merge conflicts since quartz can be finnicky about it, and it might overwrite some of your changes locally (they'll still be stored remotely, but they might be overwritten by a conflicting merge, so it's not destructive so much as it is just annoying, but still). Best practice is to make sure you always run `npx quartz sync` before and after making local changes and ensure you and your fellow collaborators aren't working on the same docs at the same time. I don't imagine this will cause many issues, but I figured it'd be best to put this note here as a warning just in case.