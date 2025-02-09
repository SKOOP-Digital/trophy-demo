samples of good ui modern design:
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digital Signage Interface</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        }

        body {
            display: flex;
            min-height: 100vh;
            background-color: #f5f5f5;
        }

        .left-sidebar {
            width: 60px;
            background-color: #1a1a2e;
            padding: 20px 0;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .left-sidebar .logo {
            color: #00bfa5;
            font-size: 24px;
            margin-bottom: 40px;
        }

        .nav-item {
            color: #6c7293;
            font-size: 20px;
            margin: 15px 0;
            cursor: pointer;
            transition: color 0.3s;
        }

        .nav-item:hover {
            color: #ffffff;
        }

        .nav-item.active {
            color: #00bfa5;
        }

        .main-content {
            flex: 1;
            padding: 20px;
            background-color: #ffffff;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .playlist-title {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .playlist-icon {
            color: #00bfa5;
            font-size: 24px;
        }

        .header h1 {
            font-size: 20px;
            color: #333;
        }

        .buttons {
            display: flex;
            gap: 10px;
        }

        .btn {
            padding: 8px 16px;
            border-radius: 4px;
            border: none;
            cursor: pointer;
            font-size: 14px;
        }

        .btn-preview {
            background-color: #ffffff;
            border: 1px solid #ddd;
            color: #333;
        }

        .btn-publish {
            background-color: #e0e0e0;
            color: #666;
        }

        .status-bar {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 20px;
            font-size: 14px;
            color: #666;
        }

        .status-pill {
            background-color: #e8f5f3;
            color: #00bfa5;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 12px;
        }

        .settings {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin-bottom: 20px;
        }

        .setting-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .setting-group label {
            font-size: 12px;
            color: #666;
        }

        .setting-group select, .setting-group input {
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 14px;
        }

        .content-list {
            border: 1px solid #eee;
            border-radius: 4px;
        }

        .content-item {
            display: flex;
            align-items: center;
            padding: 15px;
            border-bottom: 1px solid #eee;
            gap: 15px;
        }

        .content-item:last-child {
            border-bottom: none;
        }

        .content-thumbnail {
            width: 120px;
            height: 80px;
            object-fit: cover;
            border-radius: 4px;
        }

        .content-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .content-type {
            color: #00bfa5;
            font-size: 14px;
        }

        .right-sidebar {
            width: 300px;
            background-color: #ffffff;
            border-left: 1px solid #eee;
            padding: 20px;
        }

        .widget {
            display: flex;
            align-items: center;
            padding: 12px;
            margin-bottom: 10px;
            cursor: pointer;
            border-radius: 4px;
            transition: background-color 0.3s;
        }

        .widget:hover {
            background-color: #f5f5f5;
        }

        .widget-icon {
            width: 24px;
            height: 24px;
            margin-right: 12px;
            color: #00bfa5;
        }

        .widget-title {
            flex: 1;
            font-size: 14px;
            color: #333;
        }

        .widget-arrow {
            color: #666;
        }

        .help-text {
            text-align: center;
            color: #666;
            font-size: 14px;
            margin-top: 20px;
        }

        .help-link {
            color: #00bfa5;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <div class="left-sidebar">
        <div class="logo">S</div>
        <div class="nav-item active"><i class="fas fa-desktop"></i></div>
        <div class="nav-item"><i class="fas fa-image"></i></div>
        <div class="nav-item"><i class="fas fa-list"></i></div>
        <div class="nav-item"><i class="fas fa-cog"></i></div>
    </div>

    <div class="main-content">
        <div class="header">
            <div class="playlist-title">
                <i class="fas fa-list playlist-icon"></i>
                <h1>Test</h1>
            </div>
            <div class="buttons">
                <button class="btn btn-preview">Preview</button>
                <button class="btn btn-publish">Publish</button>
            </div>
        </div>

        <div class="status-bar">
            <span class="status-pill">Published</span>
            <span>2 Content items</span>
            <span>1 Screens using this playlist</span>
        </div>

        <div class="settings">
            <div class="setting-group">
                <label>Aspect Ratio</label>
                <select>
                    <option>Select Value</option>
                </select>
            </div>
            <div class="setting-group">
                <label>Default Duration</label>
                <input type="text" value="10" placeholder="Seconds">
            </div>
            <div class="setting-group">
                <label>Transition</label>
                <select>
                    <option>None</option>
                </select>
            </div>
        </div>

        <div class="content-list">
            <div class="content-item">
                <img src="test pic.jpg" alt="Test Picture" class="content-thumbnail">
                <div class="content-info">
                    <i class="fas fa-image content-type"></i>
                    <span>test pic.jpg</span>
                </div>
            </div>
            <div class="content-item">
                <img src="menu.jpg" alt="Menu" class="content-thumbnail">
                <div class="content-info">
                    <i class="fas fa-image content-type"></i>
                    <span>menu.jpg</span>
                </div>
            </div>
        </div>
    </div>

    <div class="right-sidebar">
        <div class="widget">
            <i class="fas fa-photo-video widget-icon"></i>
            <span class="widget-title">Media</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-bars widget-icon"></i>
            <span class="widget-title">Menu Builder</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-th widget-icon"></i>
            <span class="widget-title">Zones</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-clock widget-icon"></i>
            <span class="widget-title">Timer</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-clock widget-icon"></i>
            <span class="widget-title">Clock</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-cloud-sun widget-icon"></i>
            <span class="widget-title">Weather</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fab fa-youtube widget-icon"></i>
            <span class="widget-title">Youtube</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-globe widget-icon"></i>
            <span class="widget-title">Website</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-columns widget-icon"></i>
            <span class="widget-title">Templates</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-bars widget-icon"></i>
            <span class="widget-title">Menu</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-rss widget-icon"></i>
            <span class="widget-title">RSS Feed</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>
        <div class="widget">
            <i class="fas fa-desktop widget-icon"></i>
            <span class="widget-title">KIOSK</span>
            <i class="fas fa-chevron-right widget-arrow"></i>
        </div>

        <div class="help-text">
            Don't see the app you're looking for?<br>
            <a href="#" class="help-link">Submit a request.</a>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Add click handlers for buttons
            const previewBtn = document.querySelector('.btn-preview');
            const publishBtn = document.querySelector('.btn-publish');
            
            previewBtn.addEventListener('click', () => {
                alert('Preview functionality would go here');
            });
            
            publishBtn.addEventListener('click', () => {
                alert('Publish functionality would go here');
            });

            // Add click handlers for nav items
            const navItems = document.querySelectorAll('.nav-item');
            navItems.forEach(item => {
                item.addEventListener('click', () => {
                    navItems.forEach(i => i.classList.remove('active'));
                    item.classList.add('active');
                });
            });

            // Add click handlers for widgets
            const widgets = document.querySelectorAll('.widget');
            widgets.forEach(widget => {
                widget.addEventListener('click', () => {
                    alert(`${widget.querySelector('.widget-title').textContent} functionality would go here`);
                });
            });
        });
    </script>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Skoop - Digital Signage Software</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        }

        body {
            background: linear-gradient(120deg, #00BFB3 0%, #F0F0FF 100%);
            min-height: 100vh;
        }

        .top-banner {
            background-color: #00BFB3;
            color: white;
            padding: 8px;
            text-align: center;
            position: relative;
        }

        .top-banner a {
            color: white;
            text-decoration: underline;
        }

        .close-banner {
            position: absolute;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            color: white;
            cursor: pointer;
            font-size: 20px;
        }

        nav {
            background: white;
            padding: 15px 50px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            color: #00BFB3;
        }

        .nav-links {
            display: flex;
            gap: 30px;
            align-items: center;
        }

        .nav-links a {
            text-decoration: none;
            color: #666;
        }

        .btn {
            padding: 10px 20px;
            border-radius: 5px;
            border: none;
            cursor: pointer;
            font-weight: 500;
        }

        .btn-login {
            background: none;
            color: #333;
        }

        .btn-demo {
            background: #00BFB3;
            color: white;
        }

        .hero {
            display: flex;
            padding: 50px;
            max-width: 1200px;
            margin: 0 auto;
            align-items: center;
            gap: 50px;
        }

        .hero-content {
            flex: 1;
        }

        .hero-content h1 {
            font-size: 48px;
            color: #333;
            margin-bottom: 20px;
            line-height: 1.2;
        }

        .hero-content p {
            color: #666;
            font-size: 18px;
            line-height: 1.6;
            margin-bottom: 30px;
        }

        .hero-images {
            flex: 1;
            position: relative;
        }

        .device-mockup {
            width: 100%;
            max-width: 500px;
        }

        @media (max-width: 768px) {
            .hero {
                flex-direction: column;
                padding: 20px;
            }
            
            nav {
                padding: 15px 20px;
            }

            .nav-links {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="top-banner">
        Skoop has partnered with Amazon to power Amazon's new signage stick! 
        <a href="#">Click here to learn more</a>
        <button class="close-banner">×</button>
    </div>

    <nav>
        <div class="logo">SKOOP</div>
        <div class="nav-links">
            <a href="#">Features</a>
            <a href="#">Industries</a>
            <a href="#">Resources</a>
            <a href="#">Pricing</a>
            <a href="#">Contact Us</a>
            <button class="btn btn-login">Log In</button>
            <button class="btn btn-demo">Book Demo</button>
        </div>
    </nav>

    <main class="hero">
        <div class="hero-content">
            <h1>Software that supercharges digital signage</h1>
            <p>Skoop drastically improves your customer experience, automatically updates inventory, improves the effectiveness of promotions, highlights products and moves stale inventory with ease.</p>
            <button class="btn btn-demo">Book Demo</button>
        </div>
        <div class="hero-images">
            <img src="https://via.placeholder.com/500x600" alt="Digital Signage Mockups" class="device-mockup">
        </div>
    </main>

    <script>
        document.querySelector('.close-banner').addEventListener('click', function() {
            document.querySelector('.top-banner').style.display = 'none';
        });
    </script>
</body>
</html>


---

Data rough outline:

 

step/window one:

Saginaw High

Arthur Hill

Saginaw United

D1 Athletes

Pro Athletes

(Some premade video playing in background most likely)

 

step/window two:

Men’s Basketball

Women’s Basketball

Football

Baseball

Women’s Softball

Wrestling

Lacross

Women’s Track & Field

Men’s Track and field

(Something cool for background not sure what)

 

Years:

Years will only be used for teams that one a district trophy or above

(Still images in background of district trophies for background)

(click year from here to move to next window)

 

Teams roster and team phot pop up. The ability to click on player fro player stats from here.


 

Trophies photos will be 3d and we will want them spinning on screen. We will need to give them the ability to update database in the future as will as add photos and graphics or videos. Maybe even sound playing during presentation.

 

---


please create an html file (index.html) and the json file (data.json) (or update if needed) of the data which can be expanded as time goes on. 

You can use any modern html, css, or js that you'd like.  

this should be responsive to the screen, and any headers or footers should be fixed to the top or bottom of the screen.

make sure to stick to a consistent modern style
incorporate the schools colors like navy, gold, black.
make it fairly complesx and interesting.

Use the provided samples of good ui modern design for refrence on styling. 

for any images or videos, these will also be included in the json as a url to s3, but just use a placeholder text for now.

For the trophy 3d models, use a placeholder for now, and we will come back to implement this later.

create the whole html file (index.html) and sample json file (data.json) (as needed)