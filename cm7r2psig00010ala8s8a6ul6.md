---
title: "Building an Automated GitHub Repository Showcase"
seoTitle: "Automating Your GitHub Repository Display"
seoDescription: "Learn how to create an automated GitHub repository showcase, enhancing project management and portfolio presentation using GitHub Actions and Pages"
datePublished: Sun Mar 02 2025 03:30:35 GMT+0000 (Coordinated Universal Time)
cuid: cm7r2psig00010ala8s8a6ul6
slug: mygh-showcase
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1740867369964/54b1aa50-89e9-4843-93ae-45d7d5b8ae99.png
tags: css, python, web-development, html, apis, portfolio, github-actions-1, powertocloud

---

# Introduction

[![GH RepoHub Demo](https://img.shields.io/badge/Demo-Live-green?style=for-the-badge align="left")](https://vsingh55.github.io/myGH-showcase/)

### Context and Background

In today's fast-paced development environment, managing multiple projects simultaneously has become the norm rather than the exception for most developers and teams. As I found myself working on an increasing number of DevOps and cloud infrastructure projects, I encountered a significant challenge: keeping track of all my repositories and quickly navigating to the ones I needed became increasingly time-consuming and inefficient.

<mark>The pain points were clear:</mark>

1. Valuable time was being wasted navigating through GitHub's interface to locate specific repositories
    
2. There was no centralized way to showcase my technical projects and DevOps skills to potential clients or employers
    
3. Manual portfolio maintenance required constant updates whenever new infrastructure-as-code or automation projects were created
    
4. Filtering repositories by cloud technology or purpose was cumbersome with GitHub's native interface
    

These challenges led me to conceptualize the GitHub RepoHub Showcase - a solution designed to serve as a one-stop platform for organizing, filtering, and presenting my GitHub repositories in a visually appealing and efficient manner, with particular emphasis on highlighting my DevOps and cloud engineering work.

### Personal Role and Approach

As the sole developer of this project, I took a systematic approach to address these challenges. I began with a thorough assessment of what I needed from such a tool:

1. Automatic synchronization with my GitHub repositories
    
2. A clean, responsive interface that would present my DevOps and cloud infrastructure work professionally
    
3. Advanced filtering capabilities based on technologies used (Azure, AWS, GCP, Kubernetes, Terraform, etc.)
    
4. Integration with my technical blog for detailed implementation stories
    
5. A maintenance-free deployment solution that would update automatically
    

My strategic thinking process focused on leveraging GitHub's existing infrastructure - particularly GitHub Actions and GitHub Pages - to create a zero-maintenance solution. While I had experience with HTML, my CSS knowledge was limited, so I utilized AI assistance for the styling and responsive design aspects of the project. By designing a system that would run entirely within GitHub's ecosystem, I could eliminate the need for external hosting or databases while still achieving a dynamic, data-driven portfolio that showcased my infrastructure automation and cloud engineering projects.

## Technical Journey

### Problem Definition

The core technical challenge was creating a system that could:

1. Programmatically retrieve repository information from GitHub's API
    
2. Transform that data into a structured, filterable HTML interface with professional CSS styling
    
3. Deploy automatically whenever changes occurred in my repository landscape
    
4. Function without requiring a traditional server-side application
    

The limitations in existing solutions were substantial. GitHub's native interface doesn't provide customized filtering by cloud technologies or DevOps tooling. Many portfolio templates require manual updates, defeating the purpose of automation. Custom-built portfolio sites typically need dedicated hosting and maintenance.

Additionally, there were performance considerations: the solution needed to load quickly and function smoothly, even as my repository count grew over time.

### Solution Design

#### Technology Selection Rationale

After evaluating multiple approaches, I settled on a technology stack that balanced simplicity, automation, and performance - focusing on my strengths in DevOps automation:

**Python for Backend Processing:**

* Python's `requests` library provided an elegant way to interact with GitHub's REST API
    
* Python's string manipulation capabilities made HTML generation straightforward
    
* Python's widespread use in DevOps automation made it a natural choice
    

**GitHub Actions for CI/CD:**

* GitHub Actions aligns with my expertise in CI/CD pipeline development
    
* Built-in integration with the repository makes setup minimal
    
* GitHub-hosted runners provide free compute resources for the build process
    

**GitHub Pages for Hosting:**

* Zero-cost hosting directly integrated with GitHub
    
* Content delivery network ensures fast global access
    
* Automatic HTTPS configuration for security
    

**HTML & CSS with AI Assistance:**

* While comfortable with HTML structure, I leveraged AI assistance for CSS styling
    
* This approach allowed me to focus on the automation aspects where my DevOps skills were strongest
    
* The AI-assisted styling ensured a professional, responsive design despite my limited CSS experience
    

**Vanilla JavaScript for Client-Side Interactions:**

* No framework dependencies reduce maintenance burden
    
* Smaller payload sizes ensure faster page loads
    
* Full control over filtering and theme-switching algorithms
    

I deliberately avoided database dependencies, server-side rendering frameworks, and external build tools to create a solution that would be maximally resilient and minimally complex - adhering to DevOps principles of simplicity and automation.

#### Architectural Design

The architecture follows a serverless, event-driven model with four key components:

1. **Data Retrieval Module:** A Python script that authenticates with GitHub's API and fetches repository data, applying filtering rules to exclude forks, archived repositories, and explicitly excluded projects.
    
2. **HTML Generator:** A string templating system that transforms the repository data into a responsive HTML document. While I was comfortable with the HTML structure, I relied on AI assistance to generate the CSS styling needed for a professional appearance and responsive design.
    
3. **CI/CD Pipeline:** A GitHub Actions workflow triggered by pushes to the main branch, which executes the Python script and publishes the generated HTML - leveraging my core DevOps skills.
    
4. **Client-Side Application:** JavaScript code that runs in the user's browser to enable filtering, searching, and theme switching without requiring page reloads.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740851274544/efd001d9-9aaf-42d3-9780-0163ecba9d3a.png align="center")

The innovative aspect of this design is that it shifts all dynamic operations to either build-time (repository data fetching) or client-side (filtering and search), eliminating the need for a traditional backend server while still delivering a dynamic user experience - a pattern commonly employed in modern cloud-native applications.

### Implementation Challenges

During implementation, I encountered several technical obstacles:

1. **GitHub API Rate Limiting:** The GitHub API imposes rate limits that could prevent the script from fetching all repositories for users with many projects. I addressed this by implementing pagination in the API requests and adding authentication support to increase the rate limits - applying my DevOps knowledge of API integration.
    
2. **HTML Generation Complexity:** As the feature set grew, embedding HTML, CSS, and JavaScript in Python strings became unwieldy. While a template engine would be a cleaner solution, I opted for a structured string concatenation approach to maintain the zero-dependency philosophy.
    
3. **CSS Styling and Responsive Design:** Having limited prior experience with CSS, this presented a significant challenge. I utilized AI assistance to generate the appropriate CSS for responsive layouts, theming, and visual polish, while focusing my efforts on the infrastructure automation aspects where my skills were strongest.
    
4. **Theme Implementation:** Creating a dual-theme system that persisted user preferences presented challenges in both the CSS architecture and localStorage interaction. The AI assistance was particularly valuable here, helping me implement theme variables consistently across all components.
    

### Detailed Implementation Walkthrough

The implementation process followed these key steps:

1. **Setting Up the Repository Structure:** I created a clean repository with minimal files - just the Python script, requirements file, and GitHub workflow configuration - applying established DevOps practices for project organization.
    
2. **GitHub API Integration:** The core of the application is the `get_all_repositories()` function that interacts with GitHub's API:
    
    ```python
    def get_all_repositories():
        repos = []
        page = 1
        while True:
            print(f"\nFetching page {page}...")
            url = f"https://api.github.com/users/{GITHUB_USERNAME}/repos?page={page}&per_page=100"
            response = requests.get(url)
    
            if response.status_code != 200:
                print(f"API Error! Status Code: {response.status_code}")
                print(f"Response: {response.text}")
                raise Exception("Failed to fetch repositories")
    
            batch = response.json()
            if not batch:
                print("No more repositories found")
                break
    
            print(f"Found {len(batch)} repositories in this batch")
            valid_repos = [
                repo for repo in batch
                if not repo['fork'] and
                   repo['full_name'] not in EXCLUDE_REPOS and
                   not repo['archived'] and
                   not repo['private']
            ]
            print(f"After filtering: {len(valid_repos)} valid repositories")
            repos.extend(valid_repos)
            page += 1
    
        return repos
    ```
    
    This function handles pagination to retrieve all repositories, filtering out forks, archived repositories, and any specifically excluded repositories.
    
3. **HTML Generation with AI-Assisted CSS:** The `generate_html_table()` function creates the complete HTML document. While I was comfortable with the HTML structure, I leveraged AI assistance for the CSS styling to ensure a professional, responsive design:
    
    ```python
    def generate_html_table(repos):
        html = """<!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>GH RepoHub</title>
        <style>
            /* AI-assisted CSS styles here */
            /* These styles create responsive layouts and theme support */
        </style>
    </head>
    <body>
        <!-- HTML structure here -->
        <script>
            // JavaScript functionality here
        </script>
    </body>
    </html>"""
        return html
    ```
    
4. **Setting Up GitHub Actions:** The GitHub Actions workflow is defined in `.github/workflows/deploy.yml` and handles the automated build and deployment process - an area where my DevOps expertise was directly applicable:
    
    ```yaml
    name: Deploy to GitHub Pages
    
    on:
      push:
        branches: [main]
    
    jobs:
      build:
        runs-on: ubuntu-latest
        permissions:
          contents: write
          pages: write
          id-token: write
    
        steps:
        - name: Checkout code
          uses: actions/checkout@v4
    
        - name: Set up Python
          uses: actions/setup-python@v5
          with:
            python-version: '3.x'
    
        - name: Install dependencies
          run: pip install -r requirements.txt
    
        - name: Run main.py
          run: python main.py
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    
        - name: Deploy to GitHub Pages
          uses: peaceiris/actions-gh-pages@v4
          with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_dir: ./
    ```
    
5. **DevOps-Focused Filtering:** The JavaScript functions implement real-time filtering without requiring page reloads, with special attention to DevOps and cloud technologies:
    
    ```javascript
    function searchTable() {
        const searchTerm = document.getElementById('searchInput').value.toLowerCase();
        const rows = document.querySelectorAll('tbody tr');
    
        rows.forEach(row => {
            const rowText = row.textContent.toLowerCase();
            row.style.display = rowText.includes(searchTerm) ? '' : 'none';
        });
    }
    
    document.querySelectorAll('.filter-section input').forEach(checkbox => {
        checkbox.addEventListener('change', () => {
            const selectedTech = Array.from(document.querySelectorAll('.filter-section input:checked'))
                .map(cb => cb.value.toLowerCase());
    
            const rows = document.querySelectorAll('tbody tr');
            rows.forEach(row => {
                const tags = row.getAttribute('data-tags').split(',');
                const hasTech = selectedTech.length === 0 ||
                              selectedTech.some(tech => tags.includes(tech));
                row.style.display = hasTech ? '' : 'none';
            });
        });
    });
    ```
    
6. **Theme System Implementation:** The theme system persists user preferences using localStorage, with AI assistance for the CSS variables:
    
    ```javascript
    function toggleTheme() {
        const body = document.body;
        const currentTheme = body.getAttribute('data-theme');
        const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
        body.setAttribute('data-theme', newTheme);
        localStorage.setItem('theme', newTheme);
    }
    
    // Initialize theme
    const savedTheme = localStorage.getItem('theme') || 'light';
    document.body.setAttribute('data-theme', savedTheme);
    ```
    

## Outcomes and Impact

### Quantifiable Results

The implementation of the GitHub RepoHub Showcase delivered significant improvements for managing my DevOps and cloud infrastructure projects:

1. **Time Efficiency:**
    
    * Reduced repository access time from ~30 seconds of searching to ~5 seconds of filtering
        
    * Eliminated manual portfolio updates, saving approximately 1-2 hours monthly
        
    * Streamlined the process of sharing DevOps work with potential clients or employers
        
2. **Developer Experience:**
    
    * Centralized access to all cloud and infrastructure repositories with contextual information
        
    * Improved discoverability of past projects through technology filtering (AWS, Azure, GCP, Kubernetes, etc.)
        
    * Enhanced presentation of DevOps skills and capabilities
        
3. **Performance Metrics:**
    
    * Static site delivery ensures sub-second loading times
        
    * Client-side filtering provides instant results without server round-trips
        
    * Automated builds complete in under 2 minutes
        
4. **Technical Documentation:**
    
    * Integrated blog links created a seamless path to detailed implementation articles
        
    * Improved knowledge sharing and project discoverability for infrastructure-as-code repositories
        

### Technical Achievements

The project demonstrates several advanced technical practices aligned with modern DevOps principles:

1. **Serverless Architecture:** Successfully implemented a completely serverless solution with zero ongoing infrastructure costs
    
2. **CI/CD Integration:** Created a fully automated pipeline that keeps content fresh without manual intervention
    
3. **API Integration:** Developed a robust GitHub API client that handles pagination and filtering gracefully
    
4. **Responsive Design:** Successfully implemented a professional-looking interface despite limited CSS experience, by leveraging AI assistance for styling while focusing on the automation aspects
    

## Learning and Reflection

Throughout this project, I gained several key technical insights:

1. **API Design Understanding:** Working with GitHub's API enhanced my understanding of RESTful API design patterns and pagination strategies
    
2. **Static Site Generation:** I gained appreciation for the power of build-time processing for content that doesn't require real-time updates
    
3. **CSS and Frontend Development:** Through AI assistance, I was able to bridge my knowledge gap in CSS while focusing on my DevOps strengths. This collaborative approach allowed me to create a polished frontend despite limited styling experience
    
4. **DevOps Principles in Practice:** The project reinforced core DevOps principles of automation, pipeline design, and infrastructure as code
    

Some unexpected challenges included:

1. **API Rate Limiting:** I initially underestimated the impact of GitHub's API rate limits, which led to a redesign of the fetching mechanism
    
2. **Dynamic Code Generation:** Creating HTML/CSS/JS within Python strings proved more challenging than anticipated, highlighting the value of proper templating systems
    
3. **Responsive Design Requirements:** Despite AI assistance with CSS, understanding the principles of responsive design required more learning than expected
    

Future improvement opportunities include:

1. **Code Structure:** Refactoring to use a proper templating system would improve maintainability
    
2. **Additional Metrics:** Incorporating repository statistics like stars, forks, and recent activity would add valuable context for infrastructure projects
    
3. **DevOps Metric Dashboard:** Adding visualizations for deployment frequency, build times, and other DevOps metrics would enhance the showcase
    

## Conclusion

The GitHub RepoHub Showcase represents a practical solution to the common challenge of managing and presenting multiple DevOps and cloud engineering projects. By leveraging GitHub's ecosystem and moving complexity to build-time, the project achieves a maintenance-free, high-performance portfolio that automatically stays in sync with my repository collection.

The significance of this approach extends beyond just personal convenience. For DevOps teams and cloud engineering organizations managing large project portfolios, a similar approach could provide valuable visibility and organization. The serverless, event-driven architecture demonstrates how modern development tools can be composed to create solutions that are both powerful and simple to maintain.

This project highlights how effective collaboration between my core DevOps automation skills and AI-assisted CSS styling created a complete solution that I couldn't have easily built alone. By focusing on my strengths in infrastructure automation, CI/CD, and GitHub ecosystem integration, while leveraging AI for the CSS aspects where I had less experience, I was able to create a comprehensive and professional showcase.

Looking ahead, this project has laid the groundwork for further automation in my development workflow. The principles demonstrated here—build-time processing, client-side interactivity, and zero-maintenance deployment—will inform future projects where resource efficiency and automation are priorities in my continuing DevOps and cloud engineering career.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740851518964/5596d476-c846-4f06-9110-7537830558fc.png align="center")

## Technical Appendix

### Complete Technology Stack

* **Core Technologies:**
    
    * Python 3.8+
        
    * HTML5/CSS3 (with AI assistance for styling)
        
    * JavaScript (ES6)
        
* **Cloud & DevOps Technologies:**
    
    * GitHub REST API
        
    * GitHub Actions
        
    * GitHub Pages
        
* **Python Libraries:**
    
    * Requests (HTTP client)
        
    * html.escape (Security sanitization)
        
* **Development Tools:**
    
    * Git (Version control)
        
    * Python HTTP server (Local testing)
        

### Configuration Reference

The main configuration variables are located at the top of `main.py`, with emphasis on DevOps and cloud technologies:

```python
# Configuration
GITHUB_USERNAME = "vsingh55"  # GitHub username to fetch repositories for
EXCLUDE_REPOS = ["vsingh55/vsingh55"]  # Repositories to exclude
OUTPUT_FILE = "index.html"  # Output file name
BLOG_BASE_URL = "https://blogs.vijaysingh.cloud"  # Blog base URL for linking
TECH_FILTERS = [  # Technologies for filter system
    "azure", "aws", "gcp", "docker", "kubernetes", "terraform",
    "ansible", "devsecops", "gitlab", "github-actions", "ci/cd",
    "jenkins", "elk", "prometheus", "grafana", "maven", "trivy",
    "sonarqube", "linux", "git", "slack", "jira", "python",
    "shell-scripting"
]
```

### Additional Resources

* [GitHub REST API Documentation](https://docs.github.com/en/rest)
    
* [GitHub Pages Documentation](https://docs.github.com/en/pages)
    
* [GitHub Actions Documentation](https://docs.github.com/en/actions)
    
* [Live Demo](https://vsingh55.github.io/myGH-showcase/)
    
* %[https://github.com/vsingh55/myGH-showcase]