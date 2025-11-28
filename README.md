# Power BI Reports

<img width="2008" height="497" alt="image" src="https://github.com/user-attachments/assets/000ae8c7-4d09-42bb-9e52-2fde05c03308" />
<hr>

Collection of my projects, primarily (but not only) from data challenges I participated in. These personal projects motivated me to explore innovative ways to stretch Power BI's capabilities to their limits.

The challenges I took part are organised by Maven Analytics, my favourite analytical learning platform, with a great online community. Here the link to my [Analytical Portfolio](https://mavenshowcase.com/profile/681173b0-8011-704e-bdb5-614cd4fd011e) on their page with my official entries for their challenges.

In the projects' descriptions I include links, codes and tutorials so everyone can ideally try to replicate my development techniques. 

## Reports for Data Challenges

[Maven Slopes Challenge - Ski Resorts Finder [🏆 Winner]](https://github.com/SteCiu01/Power-BI-Reports/tree/main/Ski-Resorts-Finder)

[Maven Coffee Challenge [🏆 Winner]](https://github.com/SteCiu01/Power-BI-Reports/tree/main/Maven-Coffee-Challenge)

[Maven Commuter Challenge [Finalist]](https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Commuter-Challenge/README.md)

[The History of the Space Race[Finalist]](https://github.com/SteCiu01/Power-BI-Reports/tree/main/The%20History%20of%20the%20Space%20Race%20%5BFinalist%5D)

[Maven LEGO Challenge - LEGO Sets Explorer [Finalist]](https://github.com/SteCiu01/Power-BI-Reports/tree/main/Maven-LEGO-Challenge%5BFinalist%5D)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Challenge Reports</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 40px 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            color: white;
            font-size: 2.5rem;
            margin-bottom: 50px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 30px;
        }

        .card {
            background: white;
            border-radius: 16px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
            text-decoration: none;
            display: block;
        }

        .card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        .card-image {
            width: 100%;
            height: 250px;
            object-fit: cover;
            display: block;
        }

        .card-content {
            padding: 20px;
        }

        .card-title {
            font-size: 1.25rem;
            font-weight: 600;
            color: #2d3748;
            margin-bottom: 10px;
            line-height: 1.4;
        }

        .badge {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            margin-top: 10px;
        }

        .winner {
            background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
            color: #fff;
        }

        .finalist {
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            color: #2d3748;
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 2rem;
            }

            .cards-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📊 Reports for Data Challenges</h1>
        
        <div class="cards-grid">
            <a href="https://github.com/SteCiu01/Power-BI-Reports/tree/main/Ski-Resorts-Finder" class="card" target="_blank">
                <img src="https://github.com/user-attachments/assets/b9806d78-ef09-485d-9f6d-a92a24f34713" alt="Ski Resorts Finder" class="card-image">
                <div class="card-content">
                    <div class="card-title">Maven Slopes Challenge - Ski Resorts Finder</div>
                    <span class="badge winner">🏆 Winner</span>
                </div>
            </a>

            <a href="https://github.com/SteCiu01/Power-BI-Reports/tree/main/Maven-Coffee-Challenge" class="card" target="_blank">
                <img src="https://github.com/user-attachments/assets/92c723cb-d985-41f9-a107-ac3248f6ef33" alt="Maven Coffee Challenge" class="card-image">
                <div class="card-content">
                    <div class="card-title">Maven Coffee Challenge</div>
                    <span class="badge winner">🏆 Winner</span>
                </div>
            </a>

            <a href="https://github.com/SteCiu01/Power-BI-Reports/blob/main/Maven-Commuter-Challenge/README.md" class="card" target="_blank">
                <img src="https://github.com/user-attachments/assets/058336ae-a826-467f-846a-c93f84661d7e" alt="Maven Commuter Challenge" class="card-image">
                <div class="card-content">
                    <div class="card-title">Maven Commuter Challenge</div>
                    <span class="badge finalist">Finalist</span>
                </div>
            </a>

            <a href="https://github.com/SteCiu01/Power-BI-Reports/tree/main/The%20History%20of%20the%20Space%20Race%20%5BFinalist%5D" class="card" target="_blank">
                <img src="https://github.com/user-attachments/assets/cebb8afa-ded7-4e01-9535-673698af65be" alt="The History of the Space Race" class="card-image">
                <div class="card-content">
                    <div class="card-title">The History of the Space Race</div>
                    <span class="badge finalist">Finalist</span>
                </div>
            </a>

            <a href="https://github.com/SteCiu01/Power-BI-Reports/tree/main/Maven-LEGO-Challenge%5BFinalist%5D" class="card" target="_blank">
                <img src="https://github.com/user-attachments/assets/16c668c2-07f9-4cd9-853c-fff7f6758d2e" alt="Maven LEGO Challenge" class="card-image">
                <div class="card-content">
                    <div class="card-title">Maven LEGO Challenge - LEGO Sets Explorer</div>
                    <span class="badge finalist">Finalist</span>
                </div>
            </a>
        </div>
    </div>
</body>
</html>
