<script setup lang="ts">
import { ref, onMounted } from "vue";

const activeSection = ref("about");
const isDarkMode = ref(false);
const isMobileMenuOpen = ref(false);

interface Experience {
  company: string;
  role: string;
  period: string;
  location: string;
  achievements: string[];
}

interface Project {
  name: string;
  tech: string;
  url: string;
  description: string[];
}

const experiences: Experience[] = [
  {
    company: "Amazon - Kindle Unlimited",
    role: "Software Engineer - Full Stack",
    period: "Aug 2016 - Apr 2026",
    location: "Seattle, WA",
    achievements: [
      "Developed and maintained Kindle Unlimited, Prime Reading, and Comixology Unlimited, a distributed subscription platform for 16M+ users across 13 global marketplaces, powering sign-up, discovery, and borrowing experiences.",
      "Built a dynamic, in-page browsing and filtering experience, reducing discovery friction and increasing borrow conversion by 10%.",
      "Designed a traffic attribution system capturing user navigation touchpoints, enabling cross-functional teams to identify high-impact paths and prioritize funnel optimizations.",
      "Developed an ML-based cold-start recommendation system on distributed services, increasing first-borrow conversion by 5-6% for new subscribers with no prior history.",
      "Engineered an AI-driven on-call companion leveraging Claude, historical incident data, and runbooks, enabling cross-team engineers to resolve tickets faster and standardizing best-practice incident responses.",
      "Built a Claude-powered rebrand tool, automating 20k+ string updates, reducing manual effort, and ensuring brand consistency across platforms.",
      "Directed a backend migration to a modern tech stack, achieving 10% cost savings and 20% lower latency while coordinating cross-team implementation and minimizing downtime.",
      "Designed and built an automated, GDPR-compliant data deletion pipeline processing 100K+ requests/month, adopted across multiple services, eliminating manual effort, and meeting regulatory SLAs.",
      "Led accessibility and security initiatives across 10+ customer-facing surfaces and services, achieving WCAG 2.1 AA compliance, reducing accessibility defects by 80%, and preventing high-severity security and data privacy issues prior to launch.",
    ],
  },
  {
    company: "Lifx",
    role: "Software Engineer Intern",
    period: "Jun 2014 - Aug 2014",
    location: "Redwood City, CA",
    achievements: [
      "Engineered an SDK integrating LIFX smart bulbs with the Unity game engine through UDP, enabling developers to control dynamic lighting in real-time.",
      "Created a demo game showcasing adaptive lighting, illustrating real-time interaction between gameplay and smart home devices.",
    ],
  },
];

const projects: Project[] = [
  {
    name: "Timeline",
    tech: "Rust, Vue.js, TypeScript, Google Maps API",
    url: "https://github.com/twluo/timeline",
    description: [
      "Recreated the Google Maps Timeline feature for personal location tracking and place visit history.",
      "Built a Rust REST API backend that ingests GPS coordinates, resolves nearby places using Google Places API, and serves timeline data.",
      "Developed a Vue.js web client that displays daily stops as an interactive map with visit history and frequency tracking.",
      "Implemented data ingestion, place resolution, and chronological timeline generation for tracking locations throughout the day.",
      "Designed with plans for multi-device support, authentication, and mobile clients for Android and iOS.",
    ],
  },
  {
    name: "Media Player",
    tech: "Vue.js, TypeScript, Claude",
    url: "https://github.com/twluo/MediaPlayer",
    description: [
      "Architected and built a web application that consolidates media libraries from Plex and Navidrome into a unified browsing and playback experience.",
      "Created a TypeScript-based data abstraction layer to standardize media metadata across multiple third-party APIs.",
      "Developed reusable Vue components and reactive state management patterns to support scalable feature development.",
      "Implemented search, library synchronization, authentication, and playback controls across disparate media platforms.",
      "Reduced complexity for end users by providing a single interface for managing content distributed across multiple self-hosted services.",
    ],
  },
];

const skills = {
  Languages: "Java, Python, Scala, TypeScript, JavaScript, Rust, Haskell, C++, SQL",
  "Cloud & Distributed Systems": "AWS, Apache Airflow, Spring, React, Vue, SolidJS",
  "Machine Learning & AI": "Claude, Kiro, PyTorch, TensorFlow, Pandas, NumPy",
};

function scrollToSection(section: string) {
  activeSection.value = section;
  const element = document.getElementById(section);
  if (element) {
    element.scrollIntoView({ behavior: "smooth" });
  }
  // Close mobile menu after navigation
  isMobileMenuOpen.value = false;
}

function toggleMobileMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
}

function toggleDarkMode() {
  isDarkMode.value = !isDarkMode.value;
  if (isDarkMode.value) {
    document.documentElement.classList.add("dark");
    localStorage.setItem("theme", "dark");
  } else {
    document.documentElement.classList.remove("dark");
    localStorage.setItem("theme", "light");
  }
}

onMounted(() => {
  // Check for saved theme preference or use system preference
  const savedTheme = localStorage.getItem("theme");

  if (savedTheme === "dark") {
    isDarkMode.value = true;
    document.documentElement.classList.add("dark");
  } else if (savedTheme === "light") {
    isDarkMode.value = false;
    document.documentElement.classList.remove("dark");
  } else {
    // No saved preference, use system preference
    const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
    isDarkMode.value = prefersDark;
    if (prefersDark) {
      document.documentElement.classList.add("dark");
    }
  }

  // Listen for system theme changes
  const mediaQuery = window.matchMedia("(prefers-color-scheme: dark)");
  const handleChange = (e: MediaQueryListEvent) => {
    // Only auto-update if user hasn't manually set a preference
    if (!localStorage.getItem("theme")) {
      isDarkMode.value = e.matches;
      if (e.matches) {
        document.documentElement.classList.add("dark");
      } else {
        document.documentElement.classList.remove("dark");
      }
    }
  };

  mediaQuery.addEventListener("change", handleChange);
});
</script>

<template>
  <div class="app">
    <!-- Navigation -->
    <nav class="nav">
      <div class="nav-container">
        <div class="nav-brand">Tony Luo</div>
        <div class="nav-right">
          <div class="nav-links" :class="{ 'mobile-open': isMobileMenuOpen }">
            <button
              @click="scrollToSection('about')"
              :class="{ active: activeSection === 'about' }"
            >
              About
            </button>
            <button
              @click="scrollToSection('experience')"
              :class="{ active: activeSection === 'experience' }"
            >
              Experience
            </button>
            <button
              @click="scrollToSection('projects')"
              :class="{ active: activeSection === 'projects' }"
            >
              Projects
            </button>
            <button
              @click="scrollToSection('education')"
              :class="{ active: activeSection === 'education' }"
            >
              Education
            </button>
            <button
              @click="scrollToSection('skills')"
              :class="{ active: activeSection === 'skills' }"
            >
              Skills
            </button>
          </div>
          <button
            @click="toggleDarkMode"
            class="theme-toggle"
            :aria-label="isDarkMode ? 'Switch to light mode' : 'Switch to dark mode'"
          >
            <svg
              v-if="!isDarkMode"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path>
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <circle cx="12" cy="12" r="5"></circle>
              <line x1="12" y1="1" x2="12" y2="3"></line>
              <line x1="12" y1="21" x2="12" y2="23"></line>
              <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line>
              <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line>
              <line x1="1" y1="12" x2="3" y2="12"></line>
              <line x1="21" y1="12" x2="23" y2="12"></line>
              <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line>
              <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line>
            </svg>
          </button>
          <button
            @click="toggleMobileMenu"
            class="mobile-menu-toggle"
            :aria-label="isMobileMenuOpen ? 'Close menu' : 'Open menu'"
          >
            <svg
              v-if="!isMobileMenuOpen"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <line x1="3" y1="12" x2="21" y2="12"></line>
              <line x1="3" y1="6" x2="21" y2="6"></line>
              <line x1="3" y1="18" x2="21" y2="18"></line>
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <line x1="18" y1="6" x2="6" y2="18"></line>
              <line x1="6" y1="6" x2="18" y2="18"></line>
            </svg>
          </button>
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <section id="about" class="hero">
      <div class="container">
        <h1 class="hero-title">Tony Luo</h1>
        <p class="hero-subtitle">Full Stack Software Engineer</p>
        <div class="hero-contact">
          <a href="mailto:contactme@twluo.com" class="contact-link">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"
              ></path>
              <polyline points="22,6 12,13 2,6"></polyline>
            </svg>
            contactme@twluo.com
          </a>
          <a href="https://linkedin.com/in/twluo" target="_blank" class="contact-link">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"
              ></path>
              <rect x="2" y="9" width="4" height="12"></rect>
              <circle cx="4" cy="4" r="2"></circle>
            </svg>
            linkedin.com/in/twluo
          </a>
          <a href="https://github.com/twluo" target="_blank" class="contact-link">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"
              ></path>
            </svg>
            github.com/twluo
          </a>
        </div>
        <div class="hero-description">
          <p>
            Full Stack Software Engineer with 10+ years of experience building large-scale
            distributed systems at Amazon. Specialized in building user-facing products that serve
            millions of users globally.
          </p>
        </div>
      </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="section">
      <div class="container">
        <h2 class="section-title">Experience</h2>
        <div class="timeline">
          <div v-for="(exp, index) in experiences" :key="index" class="experience-card">
            <div class="experience-header">
              <div>
                <h3 class="experience-company">{{ exp.company }}</h3>
                <p class="experience-role">{{ exp.role }}</p>
              </div>
              <div class="experience-meta">
                <p class="experience-period">{{ exp.period }}</p>
                <p class="experience-location">{{ exp.location }}</p>
              </div>
            </div>
            <ul class="experience-achievements">
              <li v-for="(achievement, idx) in exp.achievements" :key="idx">
                {{ achievement }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <section id="projects" class="section section-alt">
      <div class="container">
        <h2 class="section-title">Projects</h2>
        <div class="projects-grid">
          <div v-for="(project, index) in projects" :key="index" class="project-card">
            <div class="project-header">
              <h3 class="project-name">{{ project.name }}</h3>
              <a :href="project.url" target="_blank" class="project-link">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path>
                  <polyline points="15 3 21 3 21 9"></polyline>
                  <line x1="10" y1="14" x2="21" y2="3"></line>
                </svg>
              </a>
            </div>
            <p class="project-tech">{{ project.tech }}</p>
            <ul class="project-description">
              <li v-for="(desc, idx) in project.description" :key="idx">
                {{ desc }}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- Education Section -->
    <section id="education" class="section">
      <div class="container">
        <h2 class="section-title">Education</h2>
        <div class="education-card">
          <div class="education-content">
            <div>
              <h3 class="education-school">University of California, San Diego</h3>
              <p class="education-degree">Bachelor of Science in Computer Science</p>
            </div>
            <div class="education-meta">
              <p class="education-location">San Diego, CA</p>
              <p class="education-date">Aug. 2016</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="section section-alt">
      <div class="container">
        <h2 class="section-title">Technical Skills</h2>
        <div class="skills-grid">
          <div v-for="(skillList, category) in skills" :key="category" class="skill-category">
            <h3 class="skill-category-name">{{ category }}</h3>
            <p class="skill-list">{{ skillList }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
      <div class="container">
        <p>&copy; 2026 Tony Luo. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root.dark {
  background-color: #111827;
}

.app {
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  color: #333;
  line-height: 1.6;
  transition:
    background-color 0.3s,
    color 0.3s;
}

:root.dark .app {
  background-color: #111827;
  color: #e5e7eb;
}

/* Navigation */
.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid #e5e7eb;
  z-index: 1000;
  padding: 1rem 0;
  transition:
    background-color 0.3s,
    border-color 0.3s;
}

:root.dark .nav {
  background: rgba(17, 24, 39, 0.95);
  border-bottom: 1px solid #374151;
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-brand {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1f2937;
}

:root.dark .nav-brand {
  color: #f9fafb;
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.nav-links {
  display: flex;
  gap: 2rem;
}

@media (max-width: 768px) {
  .nav-links {
    position: fixed;
    top: 69px;
    left: 0;
    right: 0;
    background: rgba(255, 255, 255, 0.98);
    backdrop-filter: blur(10px);
    flex-direction: column;
    padding: 1rem 0;
    gap: 0;
    max-height: 0;
    overflow: hidden;
    transition:
      max-height 0.3s ease-in-out,
      opacity 0.3s ease-in-out,
      visibility 0s 0.3s;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    opacity: 0;
    visibility: hidden;
  }

  :root.dark .nav-links {
    background: rgba(17, 24, 39, 0.98);
  }

  .nav-links.mobile-open {
    max-height: 400px;
    opacity: 1;
    visibility: visible;
    transition:
      max-height 0.3s ease-in-out,
      opacity 0.3s ease-in-out,
      visibility 0s 0s;
  }

  .nav-links button {
    width: 100%;
    text-align: left;
    padding: 1rem 2rem;
    border-bottom: 1px solid #e5e7eb;
  }

  :root.dark .nav-links button {
    border-bottom: 1px solid #374151;
  }

  .nav-links button:last-child {
    border-bottom: none;
  }

  .nav-links button.active::after {
    display: none;
  }

  .nav-links button.active {
    background: rgba(37, 99, 235, 0.1);
  }
}

.nav-links button {
  background: none;
  border: none;
  color: #6b7280;
  font-size: 1rem;
  cursor: pointer;
  transition: color 0.2s;
  padding: 0.5rem 0;
  position: relative;
}

.nav-links button:hover {
  color: #1f2937;
}

.nav-links button.active {
  color: #2563eb;
}

.nav-links button.active::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: #2563eb;
}

.theme-toggle {
  background: none;
  border: none;
  color: #6b7280;
  cursor: pointer;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  transition: all 0.2s;
}

.theme-toggle:hover {
  color: #1f2937;
  background: rgba(0, 0, 0, 0.05);
}

:root.dark .theme-toggle {
  color: #9ca3af;
}

:root.dark .theme-toggle:hover {
  color: #f9fafb;
  background: rgba(255, 255, 255, 0.1);
}

.mobile-menu-toggle {
  display: none;
  background: none;
  border: none;
  color: #6b7280;
  cursor: pointer;
  padding: 0.5rem;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  transition: all 0.2s;
}

.mobile-menu-toggle:hover {
  color: #1f2937;
  background: rgba(0, 0, 0, 0.05);
}

:root.dark .mobile-menu-toggle {
  color: #9ca3af;
}

:root.dark .mobile-menu-toggle:hover {
  color: #f9fafb;
  background: rgba(255, 255, 255, 0.1);
}

@media (max-width: 768px) {
  .mobile-menu-toggle {
    display: flex;
  }
}

/* Hero Section */
.hero {
  padding: 8rem 0 4rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

@media (max-width: 768px) {
  .container {
    padding: 0 1rem;
  }
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 0.5rem;
}

.hero-subtitle {
  font-size: 1.5rem;
  font-weight: 300;
  margin-bottom: 2rem;
  opacity: 0.9;
}

.hero-contact {
  display: flex;
  flex-wrap: wrap;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.contact-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: white;
  text-decoration: none;
  opacity: 0.9;
  transition: opacity 0.2s;
}

.contact-link:hover {
  opacity: 1;
}

.hero-description {
  max-width: 800px;
  font-size: 1.125rem;
  line-height: 1.8;
  opacity: 0.95;
}

/* Sections */
.section {
  padding: 5rem 0;
}

.section-alt {
  background: #f9fafb;
  transition: background-color 0.3s;
}

:root.dark .section-alt {
  background: #1a202c;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 3rem;
  color: #1f2937;
  position: relative;
  display: inline-block;
}

:root.dark .section-title {
  color: #f9fafb;
}

.section-title::after {
  content: "";
  position: absolute;
  bottom: -0.5rem;
  left: 0;
  width: 60px;
  height: 4px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 2px;
}

/* Experience */
.timeline {
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.experience-card {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition:
    transform 0.2s,
    box-shadow 0.2s,
    background-color 0.3s;
}

:root.dark .experience-card {
  background: #2d3748;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.experience-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

:root.dark .experience-card:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
}

.experience-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1.5rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.experience-company {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1f2937;
  margin-bottom: 0.25rem;
}

:root.dark .experience-company {
  color: #f9fafb;
}

.experience-role {
  font-size: 1.125rem;
  color: #6b7280;
  font-style: italic;
}

:root.dark .experience-role {
  color: #9ca3af;
}

.experience-meta {
  text-align: right;
}

.experience-period {
  font-weight: 600;
  color: #374151;
}

:root.dark .experience-period {
  color: #d1d5db;
}

.experience-location {
  color: #6b7280;
  font-size: 0.875rem;
}

:root.dark .experience-location {
  color: #9ca3af;
}

.experience-achievements {
  list-style: none;
  padding: 0;
}

.experience-achievements li {
  padding-left: 1.5rem;
  margin-bottom: 0.75rem;
  position: relative;
  color: #4b5563;
}

:root.dark .experience-achievements li {
  color: #d1d5db;
}

.experience-achievements li::before {
  content: "▹";
  position: absolute;
  left: 0;
  color: #667eea;
  font-size: 1.25rem;
}

/* Projects */
.projects-grid {
  display: grid;
  gap: 2rem;
}

.project-card {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition:
    transform 0.2s,
    box-shadow 0.2s,
    background-color 0.3s;
}

:root.dark .project-card {
  background: #2d3748;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.project-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

:root.dark .project-card:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
}

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.5rem;
}

.project-name {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1f2937;
}

:root.dark .project-name {
  color: #f9fafb;
}

.project-link {
  color: #667eea;
  transition: transform 0.2s;
  display: flex;
}

.project-link:hover {
  transform: scale(1.1);
}

.project-tech {
  color: #6b7280;
  font-size: 0.875rem;
  margin-bottom: 1rem;
  font-style: italic;
}

:root.dark .project-tech {
  color: #9ca3af;
}

.project-description {
  list-style: none;
  padding: 0;
}

.project-description li {
  padding-left: 1.5rem;
  margin-bottom: 0.75rem;
  position: relative;
  color: #4b5563;
}

:root.dark .project-description li {
  color: #d1d5db;
}

.project-description li::before {
  content: "▹";
  position: absolute;
  left: 0;
  color: #667eea;
  font-size: 1.25rem;
}

/* Education */
.education-card {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition:
    transform 0.2s,
    box-shadow 0.2s,
    background-color 0.3s;
}

:root.dark .education-card {
  background: #2d3748;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.education-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
}

:root.dark .education-card:hover {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
}

.education-content {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
}

.education-school {
  font-size: 1.5rem;
  font-weight: 700;
  color: #1f2937;
  margin-bottom: 0.5rem;
}

:root.dark .education-school {
  color: #f9fafb;
}

.education-degree {
  color: #6b7280;
  font-size: 1.125rem;
  font-style: italic;
}

:root.dark .education-degree {
  color: #9ca3af;
}

.education-meta {
  text-align: right;
}

.education-location {
  color: #6b7280;
  margin-bottom: 0.25rem;
}

:root.dark .education-location {
  color: #9ca3af;
}

.education-date {
  font-weight: 600;
  color: #374151;
}

:root.dark .education-date {
  color: #d1d5db;
}

/* Skills */
.skills-grid {
  display: grid;
  gap: 2rem;
}

.skill-category {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition:
    background-color 0.3s,
    box-shadow 0.3s;
}

:root.dark .skill-category {
  background: #2d3748;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.skill-category-name {
  font-size: 1.25rem;
  font-weight: 700;
  color: #1f2937;
  margin-bottom: 0.75rem;
}

:root.dark .skill-category-name {
  color: #f9fafb;
}

.skill-list {
  color: #4b5563;
  line-height: 1.8;
}

:root.dark .skill-list {
  color: #d1d5db;
}

/* Footer */
.footer {
  background: #1f2937;
  color: white;
  padding: 2rem 0;
  text-align: center;
  transition: background-color 0.3s;
}

:root.dark .footer {
  background: #0f172a;
}

.footer p {
  opacity: 0.8;
}

/* Responsive */
@media (max-width: 1024px) {
  .hero-title {
    font-size: 3rem;
  }

  .hero-subtitle {
    font-size: 1.375rem;
  }
}

@media (max-width: 768px) {
  .nav-container {
    padding: 0 1rem;
  }

  .nav-brand {
    font-size: 1.25rem;
  }

  .nav-right {
    gap: 0.5rem;
  }

  .hero {
    padding: 6rem 0 3rem;
  }

  .hero-title {
    font-size: 2.25rem;
  }

  .hero-subtitle {
    font-size: 1.125rem;
  }

  .hero-contact {
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
  }

  .hero-description {
    font-size: 1rem;
  }

  .section {
    padding: 3rem 0;
  }

  .section-title {
    font-size: 1.875rem;
  }

  .experience-header {
    flex-direction: column;
  }

  .experience-meta {
    text-align: left;
  }

  .experience-company {
    font-size: 1.25rem;
  }

  .experience-role {
    font-size: 1rem;
  }

  .project-name {
    font-size: 1.25rem;
  }

  .education-school {
    font-size: 1.25rem;
  }

  .education-meta {
    text-align: left;
  }

  .skill-category-name {
    font-size: 1.125rem;
  }
}

@media (max-width: 480px) {
  .hero-title {
    font-size: 1.875rem;
  }

  .hero-subtitle {
    font-size: 1rem;
  }

  .section-title {
    font-size: 1.5rem;
  }

  .experience-card,
  .project-card,
  .education-card,
  .skill-category {
    padding: 1.5rem;
  }

  .contact-link {
    font-size: 0.875rem;
  }
}
</style>
