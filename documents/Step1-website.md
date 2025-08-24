# **Project Plan: A Personal Portfolio Website and Low-Latency Trading System**

## **Executive Summary**

This document outlines a comprehensive project plan for the development and deployment of a dual-system platform. The primary objective is to construct a public-facing professional portfolio website that simultaneously serves as a control center for a private, high-performance, low-latency algorithmic trading system. This project is conceived as an advanced educational endeavor, designed to provide practical, industrial-grade experience in full-stack web development, ultra-low-latency systems engineering, and quantitative finance. The plan now defaults to professional-grade architectural principles and technologies from the outset.

The core deliverables of this project are fourfold:

1. A fully deployed, modern personal website accessible via a custom domain name.  
2. A modular, high-performance trading system, with its latency-critical components developed in C++, deployed on a dedicated bare-metal server to eliminate virtualization overhead.  
3. A high-fidelity backtesting framework capable of accurately simulating trading strategies against historical tick-by-tick data, accounting for critical factors like order queue position and network latency.  
4. An integrated, real-time web dashboard hosted on the personal website, providing comprehensive monitoring and control over the trading system's operations.

Key strategic decisions have been made to ensure the project's success within its technical and financial parameters. The frontend website will be built using the **Astro** framework, leveraging its "islands architecture" to host a dynamic dashboard built with **React** components. This approach ensures a high-performance static site while accommodating complex, real-time user interface requirements.1 The high-frequency trading (HFT) system will be architected as a set of decoupled microservices, using modern

**C++** for performance-critical components and **ZeroMQ** for lightweight, high-speed inter-process communication.3 To achieve the lowest possible latency, the system will employ

**kernel-bypass networking** using the **Data Plane Development Kit (DPDK)**, allowing the market data handler to ingest data directly from the network interface card (NIC), avoiding the overhead of the operating system's kernel.131 For other high-performance I/O tasks, such as logging, the system will utilize

**io\_uring** to minimize system call overhead.134 To facilitate realistic yet affordable operation, the system will interface with

**Alpaca** for brokerage services and **Polygon.io** for market data, chosen for their developer-friendly, low-cost APIs.5 Deployment will be on a

**bare-metal server**, prioritizing raw performance and predictable latency over the flexibility of cloud virtual machines.137

The project roadmap is structured into distinct, iterative phases. It begins with the foundational website, proceeds with the incremental development of the HFT system's components and backtester, and culminates in their integration, bare-metal deployment, and end-to-end testing. This phased approach prioritizes the early establishment of a functional system, which can be progressively refined and enhanced over time.

## **Part I: The Public Showcase \- Your Personal Website**

This initial part of the project focuses on establishing a robust and professional online presence. Given the user's lack of prior web development experience, the plan emphasizes modern tools and frameworks that simplify the development process while delivering a high-quality, performant, and visually appealing final product.

### **1.1 Framework Selection: Astro for Performance and Flexibility**

The choice of a web framework is a foundational decision that impacts performance, developer experience, and cost. For this project, a static-first approach is paramount.

Primary Recommendation: Astro  
Astro is a modern static site generator that is exceptionally well-suited for content-heavy websites such as portfolios, blogs, and marketing sites.2 Its core philosophy is "zero JavaScript by default," meaning it renders components to static HTML at build time, shipping the absolute minimum amount of client-side JavaScript necessary. This results in websites that load with remarkable speed, a critical factor for user retention and creating a professional first impression.2  
The most compelling feature of Astro for this project is its "Islands Architecture".2 This innovative approach allows developers to build a website that is predominantly static HTML, while specific, designated components—or "islands"—can be rendered as fully interactive client-side applications using popular UI frameworks like React or Vue. This hybrid model is perfectly aligned with the project's requirements: a largely static portfolio site that contains a highly dynamic, real-time trading dashboard. The portfolio pages remain lightweight and fast, while the dashboard island receives the necessary JavaScript to become a full-featured application.

Comparative Analysis: Astro vs. Next.js  
Next.js is a powerful and popular full-stack framework based on React, widely used for building dynamic web applications.7 While it is an excellent choice for complex, server-driven applications, its default reliance on client-side rendering and hydration can be suboptimal for a content-focused portfolio. This can lead to larger initial JavaScript bundles and potentially slower page loads compared to Astro's static-first approach.1 For the specific use case of a mostly static site hosting a single, complex real-time component, Astro offers a more optimized balance of performance and capability, avoiding the overhead of a full single-page application (SPA) where it is not needed.7  
The selection of a static-first framework like Astro is not merely a technical preference; it is a strategic decision that directly enables the project to meet its primary financial constraint. Modern hosting platforms such as Vercel and Netlify offer exceptionally generous free tiers for static websites, which include features like a global CDN, automatic HTTPS, and continuous deployment.13 A framework that generates a truly static site can fully leverage these free tiers. In contrast, frameworks that require a server runtime for features like server-side rendering (SSR) often push projects into paid hosting plans more quickly. Therefore, the choice of Astro allows the project to be deployed on Vercel's free tier, effectively reducing the website's static hosting cost to $0 and ensuring adherence to the project's strict budget.

### **1.2 Core Website Implementation: A Step-by-Step Guide**

This section provides a clear path for building the website from the ground up.

* **Environment Setup:** The first step is to prepare the local development environment. This involves installing Node.js (which includes the npm package manager) and a code editor like Visual Studio Code. Recommended VS Code extensions, such as the official Astro extension and Tailwind CSS IntelliSense, will significantly improve the development experience by providing syntax highlighting and autocompletion.18  
* **Project Initialization:** The project begins by running the npm create astro@latest command in the terminal. This command launches a setup wizard that guides the user through the initial project configuration.19 For this project, selecting the "Empty" template is recommended, as it provides a minimal starting point that is ideal for learning the framework's core concepts from scratch.  
* **Creating Pages and Routing:** Astro employs an intuitive file-based routing system. Any .astro file created within the src/pages/ directory automatically becomes a page on the website.9 The plan involves creating the essential pages for a professional portfolio:  
  * src/pages/index.astro: The homepage.  
  * src/pages/about.astro: A page for a personal biography and skills.  
  * src/pages/projects/index.astro: A landing page to showcase various projects.  
  * src/pages/projects/hft-dashboard.astro: The placeholder page that will eventually host the trading dashboard.  
* **Building with Components and Layouts:** To promote code reuse and maintainability, the plan introduces Astro's component and layout paradigms.9 A main  
  Layout.astro file will be created in src/layouts/ to contain the common HTML structure, including the \<head\> section, header, and footer, which will wrap all other pages. Reusable UI elements, such as a navigation bar, will be built as individual .astro components in src/components/.  
* **Styling with Tailwind CSS:** For styling, the plan recommends **Tailwind CSS**. Its utility-first approach allows for rapid development of modern, responsive designs directly within the HTML markup, which is particularly beneficial for those without extensive CSS experience.25 Tailwind can be easily added to the Astro project by running the command  
  npx astro add tailwind.

### **1.3 Deployment and Public Access: Going Live**

With the website structure in place, the next step is to make it publicly accessible.

* **Domain Name Registration:** A custom domain name is essential for a professional online presence.  
  * **Cost-Effective Options:** While many registrars exist, **Cloudflare Registrar** is recommended as it offers at-cost domain registration, meaning it does not add a markup to the wholesale price set by the registry.26 A standard  
    .com domain typically costs between $10 and $20 per year, which is well within the project's budget.27  
  * **Process:** The process involves searching for an available domain name on the registrar's website and completing the purchase.  
* **Hosting Platform Selection: Vercel**  
  * **Rationale:** Vercel is the premier platform for deploying modern web applications built with frameworks like Astro.29 Its key feature is a seamless integration with GitHub, which enables a Continuous Integration/Continuous Deployment (CI/CD) workflow. Every time code is pushed to the GitHub repository, Vercel automatically triggers a new build and deployment, ensuring the live site is always up to date.30  
  * **Cost Analysis:** Vercel's "Hobby" plan is free and provides all the necessary features for this project, including 100 GB of monthly bandwidth, a global Content Delivery Network (CDN) for fast loading times worldwide, and automatic HTTPS for security. This makes the static hosting cost for the website effectively zero.13  
* **Step-by-Step Deployment Guide:**  
  1. **Version Control:** Initialize a Git repository in the project folder and push the code to a new repository on **GitHub**.  
  2. **Vercel Account:** Sign up for a **Vercel** account, using the option to authenticate with GitHub.  
  3. **Import Project:** From the Vercel dashboard, import the newly created GitHub repository. Vercel's build system will automatically detect that it is an Astro project and configure the necessary build commands and output directory settings.33  
  4. **Deploy:** Initiate the first deployment. Vercel will build the static site and assign it a unique .vercel.app URL for immediate access.  
  5. **Connect Custom Domain:** In the Vercel project settings, navigate to the "Domains" section. Add the custom domain purchased from Cloudflare. Vercel will provide two nameservers. The final step is to log in to the Cloudflare dashboard and update the domain's DNS settings to use the nameservers provided by Vercel. DNS propagation may take some time, after which the website will be live at the custom domain.34

---