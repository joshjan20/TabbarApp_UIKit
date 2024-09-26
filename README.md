# Tabbar with Swift UIKit
To add a `UITableView` below the video in `HomeViewController`, you can divide the screen such that the video player occupies the top half, and the table view takes up the bottom half. We'll use Auto Layout to achieve this cleanly, ensuring the video player and table view are responsive to different screen sizes.

Here's how you can modify the `HomeViewController` to include a `UITableView` below the video.

### Step-by-Step Implementation:

1. **Add the TableView**: 
   We'll add a `UITableView` below the video player and implement a simple table with some data.

2. **Set Constraints**:
   Use `NSLayoutConstraint` to pin the table view below the video player, ensuring it takes up the bottom half of the screen.

### Updated `HomeViewController.swift` with `UITableView`:

```swift
import UIKit
import AVKit
import AVFoundation

class HomeViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    
    var player: AVPlayer!
    var playerViewController: AVPlayerViewController!
    var tableView: UITableView!
    
    // Dummy data for the table view
    let tableData = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"]
    
    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .white
        
        // Set up video player and table view
        setupVideoPlayer()
        setupTableView()
    }
    
    // Function to set up the video player at the top half of the screen
    func setupVideoPlayer() {
        if let videoURL = Bundle.main.url(forResource: "sampleVideo", withExtension: "mp4") {
            player = AVPlayer(url: videoURL)
            playerViewController = AVPlayerViewController()
            playerViewController.player = player
            
            // Add the playerViewController as a child
            addChild(playerViewController)
            playerViewController.view.translatesAutoresizingMaskIntoConstraints = false
            view.addSubview(playerViewController.view)
            playerViewController.didMove(toParent: self)
            
            // Set up auto layout constraints for the video player
            NSLayoutConstraint.activate([
                playerViewController.view.topAnchor.constraint(equalTo: view.topAnchor),
                playerViewController.view.leadingAnchor.constraint(equalTo: view.leadingAnchor),
                playerViewController.view.trailingAnchor.constraint(equalTo: view.trailingAnchor),
                playerViewController.view.heightAnchor.constraint(equalTo: view.heightAnchor, multiplier: 0.5)  // Video takes up the top half
            ])
            
            // Start playing the video
            player.play()
        } else {
            print("Video not found! Check the file name and make sure it's added to the project.")
        }
    }
    
    // Function to set up the table view at the bottom half of the screen
    func setupTableView() {
        tableView = UITableView()
        tableView.translatesAutoresizingMaskIntoConstraints = false
        tableView.dataSource = self
        tableView.delegate = self
        
        view.addSubview(tableView)
        
        // Set up auto layout constraints for the table view
        NSLayoutConstraint.activate([
            tableView.topAnchor.constraint(equalTo: playerViewController.view.bottomAnchor),  // Below the video player
            tableView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            tableView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor)  // Bottom of the screen
        ])
    }
    
    // MARK: - UITableViewDataSource methods
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return tableData.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell(style: .default, reuseIdentifier: "cell")
        cell.textLabel?.text = tableData[indexPath.row]
        return cell
    }
    
    // MARK: - UITableViewDelegate methods
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("Selected \(tableData[indexPath.row])")
    }
}
```

### Explanation:

1. **Video Player (`AVPlayerViewController`)**:
   - The video player is set up as before and constrained to the top half of the screen using `NSLayoutConstraint`.
   - `heightAnchor.constraint(equalTo: view.heightAnchor, multiplier: 0.5)` ensures that the video takes up exactly half the height of the screen.

2. **Table View (`UITableView`)**:
   - A `UITableView` is created and added below the video player.
   - The table view's constraints are set so that it starts directly below the video player and takes up the remaining half of the screen.
   - `UITableViewDataSource` and `UITableViewDelegate` protocols are implemented for populating and interacting with the table.

3. **Dummy Data**:
   - A simple array (`tableData`) is used to populate the table with five rows.
   - You can replace this data with whatever dynamic data you need later.

4. **Auto Layout**:
   - We use `translatesAutoresizingMaskIntoConstraints = false` to enable Auto Layout and set the constraints programmatically.
   - The table view is pinned directly below the video player (`playerViewController.view.bottomAnchor`) and fills the rest of the screen.

### Running the App:

- When you run the app, the video will automatically play in the top half of the screen.
- Below the video, there will be a simple table view with items like "Item 1", "Item 2", etc.
- Selecting an item in the table will print the selected item in the console.

### Customization:

- You can add more sophisticated cells to the `UITableView` by customizing the `UITableViewCell` or using a custom cell class.
- For loading dynamic data into the table view, replace the `tableData` array with your data source (e.g., fetched from an API).

Let me know if you run into any issues or need further customization! 😊
