# Personal Presentation Website

Welcome to the Project Name repository! This project uses [Hugo](https://gohugo.io/) to generate a static website that is easy to maintain and deploy. Below you'll find all the information you need to get started, manage your domains, and understand the basics of this project.

## Getting Started with Hugo

Hugo is a fast and modern static site generator that makes it easy to build websites without the need for a database or server-side scripting.

### Prerequisites

- Git installed on your local machine
- Other installations: See `commands.md` file

### Installation

1. Clone the repository to your local machine:

   ```bash
   git clone URL_OF_YOUR_REPOSITORY
   ```

2. Navigate to the project directory:

   ```bash
   cd project-name
   ```

3. Run Hugo to generate the static files:

   ```bash
   hugo
   ```

## Deploying Your Site

After generating your static site with Hugo, you can deploy it to GitHub Pages for free hosting.

### Steps to Deploy on GitHub Pages

1. Push the generated files to your GitHub repository.
2. Go to your repository settings on GitHub and find the 'Pages' section.
3. Select the branch and folder where your static files are located.
4. Save the settings, and GitHub will provide you with a URL to access your site.

## Continuous Integration (CI) with GitHub Actions

GitHub Actions can automate the deployment process every time you push new changes to your repository.

### Setting Up CI

1. Create a `.github/workflows` directory in your repository.
2. Add a workflow file (e.g., `main.yml`) with the necessary steps to install Hugo and build your site.
3. Commit and push the workflow file to your repository to enable GitHub Actions.

## Domain Management

This project uses custom domains for personal branding and organization.

### Subdomains

- Subdomains will be used to separate different sections of the site, such as:
  - `personal-blog.domain.com`
  - `professional-blog.domain.com`
  - `other-section.domain.com`

### Configuring Subdomains

1. Set up DNS records (A or CNAME) for each subdomain in your domain provider's control panel.
2. Point each subdomain to the corresponding hosting service where your content is located.

## Other things to configure

1. **Gravatar**: Add a Gravatar image to your site to personalize it.
2. **Social Media Links**: Add links to your social media profiles for easy access.
3. **Contact Form**: Implement a contact form for visitors to reach out to you.

## Conclusion

This README provides a general overview of the project setup and deployment. For more detailed instructions, please refer to the official Hugo documentation and GitHub Pages guidelines.

Thank you for visiting this repository, and happy coding!
