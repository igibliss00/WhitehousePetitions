# Whitehouse Petitions

This app demonstrates the use of fetching a JSON data type from a remote data source and decode it using JSONDecoder().  It uses the Tab Bar Controller for navigation between two view controllers.  The content from selecting a cell on the table view is shown through a custom HTML format in WKWebView.

<img src="https://github.com/igibliss00/Whitehouse-Petitions/blob/master/README_assets/1.png" width="400">

<img src="https://github.com/igibliss00/Whitehouse-Petitions/blob/master/README_assets/2.png" width="400">

## Installing

```
git clone https://www.github.com/igibliss00/WhitehousePetitions.git
```

## Features

### JSONDecoder()

This project uses JSONDecoder() to decode the instance of JSON from a remote server.  The instance of JSONDecoder calls the decode() method convert the JSON data into a Petitions object which is a struct we’ve created that conforms to the Codable protocol.  This decode() method takes in two parameters.  First parameter is “Petitions.self”, which is referring to the “Petitions” struct itself and not the instance of the struct.  The second parameter is the data type we’re converting from, which is JSON in our case.

### Instantiating a View Controller

A view controller needs to be instantiated before navigating to it.  There are different ways of instantiating a view controller in Swift.  First of which is from the storyboard.  If you have created the view controller from Main.storyboard, you can use the identifier you’ve created for yourself for that view controller as the parameter and instantiate it.

```
if let vc = storyboard?.instantiateViewController(identifier: "Detail") as? DetailViewController {
  // do something with “vc”
}
```
In the current project, we haven’t defined the view controller in the storyboard, but only in code, which makes it a free-floating class.  Therefore, instead of going through the storyboard, we can directly instantiate it:

```
let vc = DetailViewController()
```

### Data(contentsOf: )

This project uses the Data type, which is one of Swift’s fundamental data types that’s a lower level than Int or String.  It can hold any binary data, not just limited to string or integer, which is appropriate for the current project that’s fetching a JSON data type over the web. 

```
if let url = URL(string: urlString) {
    if let data = try? Data(contentsOf: url) {
        parse(json: data)
    }
}
```

### WKWebView

This project uses the web view to render the fetched data.  A web view is appropriate for rendering data with complex content, which is the case with the current JSON data type.  More specifically, we use a custom HTML in order to make the design fill up the entire viewport as well as to enlarge the font.

```

let html = """
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style> body { font-size: 150%; } </style>
</head>
<body>
\(detailItem.body)
</body>
</html>
"""

webView.loadHTMLString(html, baseURL: nil)
```

### Adding a Tab Bar Controller

In AppDelegate.swift:
```
if let tabBarController = window?.rootViewController as? UITabBarController {
    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let vc = storyboard.instantiateViewController(withIdentifier: "NavController")
    vc.tabBarItem = UITabBarItem(tabBarSystemItem: .topRated, tag: 1)
    tabBarController.viewControllers?.append(vc)
}
```

Since we already have a navigation controller on storyboard, we create a new instance of it from storyboard.  Then, we add a new “UITabBarItem” to this newly created instance.  This instance is, in turn, appended to the Tab Bar Controller since the navigation controller is wrapped in tab bar controller. 
