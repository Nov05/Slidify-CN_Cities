# Slidify-CN_Cities

The R Slidify representation contains two pages, one cover page and one page with a Plotly interactive map. 

You can click the **"Page Up/Down"** key (or **Arrow** key) to go to the previous/next page.

Visit the page here: https://nov05.github.io/Slidify-CN_Cities/

<br>

### Notes

It took me tremendous effort to solve two major issues:

1. The representation wasn't able to display the Plotly map. Rather, a black map was displayed. I referred to [this post]( https://stackoverflow.com/questions/34860207/adjust-the-size-of-plotly-charts-in-slidify) to solve this issue. The key is to contain the map as a widget on the htmal page. I added the following code to my RMarkdown file.

```
htmlwidgets::saveWidget(as_widget(p), "p.html")
cat('<iframe src="./p.html" width=100% height=100% allowtransparency="true"> </iframe>')
```

2. The representation wasn't able to be published on Github. I referred to [this post](https://stackoverflow.com/questions/23145621/how-to-publish-pages-on-github) to solve this one. Basically I moved the local project folder to another directory, then use git command to clone the empty Github project to recreate the local project folder, then push the files/folders that I wanted to upload to Github into that folder, and push the new added files to Github.

  

