<template>
  <div id="app" :style="{ background: 'linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), #1a1a2e url(/background.jpg) center/cover no-repeat fixed', backgroundSize: 'cover', backgroundPosition: 'center', backgroundAttachment: 'fixed' }">
    <nav class="navbar">
      <div class="nav-container">
        <ul class="nav-links">
          <li><a href="#home">Home</a></li>
          <li><a href="#about">About</a></li>
          <li><a href="#experience">Experience</a></li>
          <li><a href="#projects">Projects</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
      </div>
    </nav>

    <!-- Home Section -->
    <section id="home" class="hero">
      <div class="hero-content">
        <h1>Arushi Yana Thakur</h1>
        <h2 class="typing-text">
          {{ displayedText }}<span class="cursor">|</span>
        </h2>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="section">
      <div class="container">
        <h2 class="section-title">ABOUT ME</h2>
        <div class="about-layout">
          <!-- Profile Picture on Left with Resume Button -->
          <div class="profile-section">
            <img :src="'/headshot.jpg'" alt="Profile" class="profile-image" />
            <button @click="toggleResumeModal" class="resume-btn-below">
              {{ showResumeModal ? 'Close Preview' : 'View Resume' }}
            </button>
            <a
              href="/Resume_ArushiYanaThakur.pdf"
              download
              class="download-btn-below"
              v-if="showResumeModal"
            >
              Download PDF
            </a>
          </div>

          <!-- Text Cards Grid on Right with Navigation -->
          <div class="cards-container">
            <button
              @click="previousCards"
              class="nav-arrow nav-arrow-left"
              :disabled="currentCardSet === 0"
            >
              ‹
            </button>

            <div class="info-cards-grid">
              <div
                v-for="(card, index) in visibleCards"
                :key="card.id"
                class="info-card"
                :style="{ animationDelay: `${index * 0.1}s` }"
              >
                <span class="card-emoji">{{ card.emoji }}</span>
                <div class="card-content">
                  <h3>{{ card.title }}</h3>
                  <p>{{ card.description }}</p>
                </div>
              </div>
            </div>

            <button
              @click="nextCards"
              class="nav-arrow nav-arrow-right"
              :disabled="currentCardSet === totalCardSets - 1"
            >
              ›
            </button>
          </div>
        </div>
      </div>

      <!-- Resume Modal -->
      <div
        v-if="showResumeModal"
        class="resume-modal"
        @click="closeResumeModal"
      >
        <div class="modal-content" @click.stop>
          <button class="close-btn" @click="closeResumeModal">&times;</button>
          <iframe
            :src="'/Resume_ArushiYanaThakur.pdf'"
            class="resume-iframe"
          ></iframe>
        </div>
      </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="section">
      <div class="container">
        <h2 class="section-title">EXPERIENCE</h2>
        <div class="timeline">
          <div
            class="timeline-item"
            v-for="(experience, index) in experiences"
            :key="index"
          >
            <div class="timeline-dot"></div>
            <div class="timeline-content">
              <div class="timeline-date">{{ experience.date }}</div>
              <div class="timeline-title">{{ experience.title }}</div>
              <div class="timeline-company">{{ experience.company }}</div>
              <p>{{ experience.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <section id="projects" class="section">
      <div class="container">
        <h2 class="section-title">PROJECTS</h2>
        <div class="projects-grid">
          <div
            class="project-card"
            v-for="(project, index) in projects"
            :key="index"
          >
            <div class="project-content">
              <h3 class="project-title">{{ project.title }}</h3>
              <p class="project-tech">{{ project.techStack }}</p>
              <div class="project-links">
                <a :href="project.githubUrl" target="_blank">GitHub</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="section">
      <div class="container">
        <h2 class="section-title">CONTACT</h2>
        <div class="contact-content">
          <p
            style="
              font-size: 1.2rem;
              color: white;
              margin-bottom: 3rem;
              text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
              text-align: center;
            "
          >
            I'm always interested in hearing about new opportunities and
            exciting projects. Feel free to reach out if you'd like to connect!
          </p>
          <div class="contact-cards">
            <div class="contact-card">
              <div class="contact-card-content">
                <h3>Email</h3>
                <a href="mailto:arushiyanathakur@gmail.com">arushiyanathakur@gmail.com</a>
              </div>
            </div>
            <div class="contact-card">
              <div class="contact-card-content">
                <h3>LinkedIn</h3>
                <a href="https://www.linkedin.com/in/arushiythakur/" target="_blank">linkedin.com/in/arushiythakur</a>
              </div>
            </div>
            <div class="contact-card">
              <div class="contact-card-content">
                <h3>GitHub</h3>
                <a href="https://github.com/Arushiyt" target="_blank">github.com/Arushiyt</a>
              </div>
            </div>
            <div class="contact-card">
              <div class="contact-card-content">
                <h3>Location</h3>
                <span>Dallas, TX</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Copyright Section -->
    <footer class="footer">
      <div class="container">
        <p class="copyright">© 2024 Arushi Yana Thakur. All rights reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      displayedText: '',
      texts: ['Software Developer'],
      currentTextIndex: 0,
      isDeleting: false,
      typingSpeed: 150,
      deletingSpeed: 75,
      pauseTime: 3000,
      typeTimeout: null,
      showResumeModal: false,
      currentCardSet: 0,
      cardsPerSet: 4,
      cards: [
        {
          id: 1,
          emoji: '👩‍💻',
          title: 'Software Developer',
          description:
            "TCU grad, passionate about tech that's practical + beautiful.",
        },
        {
          id: 2,
          emoji: '⚡',
          title: 'Tech Stack',
          description:
            'JavaScript, Python, Java, SQL, Vue.js, SpringBoot, MongoDB, AWS.',
        },
        {
          id: 3,
          emoji: '🎨',
          title: 'Front-End & UI/UX ',
          description:
            'I mix design + usability to craft engaging experiences.',
        },
        {
          id: 4,
          emoji: '📱',
          title: 'Product Design & Management',
          description: 'Excited about shaping products end-to-end.',
        },
        {
          id: 5,
          emoji: '🎶',
          title: 'Musician at Heart',
          description:
            'Singer, songwriter, and guitarist; music inspires my problem-solving.',
        },
        {
          id: 6,
          emoji: '🌅',
          title: 'Global Perspective',
          description:
            'Born and raised in Assam, India; Currently living in Dallas, TX.',
        },
        {
          id: 7,
          emoji: '📚',
          title: 'Lifelong Learner',
          description:
            'Always experimenting, learning new tools, and growing in tech.',
        },
        {
          id: 8,
          emoji: '🤝',
          title: 'Team Player',
          description:
            'Thrive in collaboration, leadership, and clear communication.',
        },
      ],
      experiences: [
        {
          date: 'August 2025 - Present',
          title: 'Software Developer',
          company: 'Corevation',
          description:
            'Led front-end development and product enhancements using Vue.js and Python, improving content scheduling efficiency, streamlining approvals, and integrating AI-powered features to boost user engagement across platforms.',
        },
        {
          date: 'August 2024 - May 2025',
          title: 'Software Developer / Product Manager',
          company: 'Trailspur Capital Partners / Texas Christian University',
          description:
            'Engineered an automated Vue.js/AWS Lambda/Supabase platform to integrate commercial real estate data, streamline workflows, and increase market intelligence efficiency by 65%.',
        },
        {
          date: 'August 2023 - May 2025',
          title: 'Community Manager',
          company: 'Texas Christian University',
          description:
            'Supervised 10 assistants and managed a 350+ student community, driving a 65% increase in engagement and satisfaction.',
        },
        {
          date: 'January 2022 - October 2022',
          title: 'IT Assistant',
          company: 'Texas Christian University',
          description:
            'Assisted with IT operations, including network maintenance, hardware repairs, and software troubleshooting, ensuring smooth campus-wide support for students and staff.',
        },
      ],
      projects: [
        {
          title: 'Book Management System 1',
          techStack: 'Python, SQL',
          githubUrl: 'https://github.com/Arushiyt/PythonBookManager',
        },
        {
          title: 'Hogwarts Artifacts',
          techStack: 'SpringBoot, RESTful APIs, Java',
          githubUrl: 'https://github.com/Arushiyt/hogwarts-artifacts-online',
        },
        {
          title: 'Book Management System 2',
          techStack: 'Python, MongoDB',
          githubUrl: 'https://github.com/Arushiyt/book-manager-Mongodb',
        },
        {
          title: 'Rock Paper Scissors',
          techStack: 'Javascript, HTML, CSS',
          githubUrl: 'https://github.com/Arushiyt/rockpaperscissor',
        },
      ],
    };
  },
  methods: {
    typeText() {
      const currentText = this.texts[0]; // Always use the first (and only) text

      if (this.isDeleting) {
        this.displayedText = currentText.substring(
          0,
          this.displayedText.length - 1
        );
      } else {
        this.displayedText = currentText.substring(
          0,
          this.displayedText.length + 1
        );
      }

      // Check if we need to change state
      if (!this.isDeleting && this.displayedText === currentText) {
        // Finished typing, wait then start deleting
        this.typeTimeout = setTimeout(() => {
          this.isDeleting = true;
          this.typeText();
        }, this.pauseTime);
      } else if (this.isDeleting && this.displayedText === '') {
        // Finished deleting, start typing again
        this.isDeleting = false;
        this.typeTimeout = setTimeout(() => {
          this.typeText();
        }, 500);
      } else {
        // Continue current operation
        const speed = this.isDeleting ? this.deletingSpeed : this.typingSpeed;
        this.typeTimeout = setTimeout(() => {
          this.typeText();
        }, speed);
      }
    },
    toggleResumeModal() {
      this.showResumeModal = !this.showResumeModal;
    },
    closeResumeModal() {
      this.showResumeModal = false;
    },
    previousCards() {
      if (this.currentCardSet > 0) {
        this.currentCardSet--;
      }
    },
    nextCards() {
      if (this.currentCardSet < this.totalCardSets - 1) {
        this.currentCardSet++;
      }
    },
  },
  computed: {
    totalCardSets() {
      return Math.ceil(this.cards.length / this.cardsPerSet);
    },
    visibleCards() {
      const startIndex = this.currentCardSet * this.cardsPerSet;
      const endIndex = startIndex + this.cardsPerSet;
      return this.cards.slice(startIndex, endIndex);
    },
  },
  beforeUnmount() {
    if (this.typeTimeout) {
      clearTimeout(this.typeTimeout);
    }
  },
  mounted() {
    this.typeText();
    // Smooth scrolling for navigation links
    const navLinks = document.querySelectorAll('.nav-links a');
    navLinks.forEach((link) => {
      link.addEventListener('click', (e) => {
        e.preventDefault();
        const targetId = link.getAttribute('href').substring(1);
        const targetSection = document.getElementById(targetId);
        if (targetSection) {
          const navHeight = document.querySelector('.navbar')?.offsetHeight || 65;
          const elementPosition = targetSection.offsetTop - navHeight;
          window.scrollTo({
            top: elementPosition,
            behavior: 'smooth',
          });
        }
      });
    });

    // Active navigation highlighting
    window.addEventListener('scroll', () => {
      const sections = document.querySelectorAll('section[id]');
      const navLinks = document.querySelectorAll('.nav-links a');

      let current = '';
      const navHeight = document.querySelector('.navbar')?.offsetHeight || 65;
      sections.forEach((section) => {
        const sectionTop = section.offsetTop;
        const sectionHeight = section.clientHeight;
        if (scrollY >= sectionTop - navHeight - 50) {
          current = section.getAttribute('id');
        }
      });

      navLinks.forEach((link) => {
        link.classList.remove('active');
        if (link.getAttribute('href') === `#${current}`) {
          link.classList.add('active');
        }
      });
    });
  },
};
</script>
