# Responsive_Personal_Blog
Writers need a simple, responsive platform to publish and manage  posts. Traditional CMS platforms can be too complex for basic  blogging needs. This project builds a clean, mobile-friendly blog  template with posts rendered dynamically from JSON or local data.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Personal Blog</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .post-content {
            max-height: 120px;
            overflow: hidden;
            position: relative;
            transition: max-height 0.5s ease-in-out;
        }
        .post-content.expanded {
            max-height: 1000px; /* Large enough to show full content */
        }
        .post-content::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 40px;
            background: linear-gradient(to bottom, transparent, white);
        }
        .post-content.expanded::after {
            display: none;
        }
        .read-more-btn {
            cursor: pointer;
            color: #3b82f6; /* Tailwind's blue-500 */
        }
        .read-more-btn:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <!-- Header -->
    <header class="bg-white shadow-sm sticky top-0 z-10">
        <div class="container mx-auto px-6 py-4 flex justify-between items-center">
            <h1 class="text-2xl md:text-3xl font-bold text-gray-900">Prabhas Holland's Blog</h1>
            <nav>
                <a href="#" class="text-gray-600 hover:text-blue-500 mx-2">Home</a>
                <a href="#" id="about-link" class="text-gray-600 hover:text-blue-500 mx-2">About</a>
                <a href="mailto:prabhas.holland.in@gmail.com" class="text-gray-600 hover:text-blue-500 mx-2">Contact</a>
            </nav>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-6 py-8 md:py-12">
        <div class="max-w-3xl mx-auto">
            <h2 class="text-3xl md:text-4xl font-bold mb-8 text-center">Latest Posts</h2>

            <!-- Blog Posts Container -->
            <div id="posts-container" class="space-y-10">
                <!-- Posts will be dynamically inserted here -->
            </div>

             <!-- Loading Spinner -->
             <div id="loading" class="text-center py-10">
                <svg class="animate-spin h-8 w-8 text-blue-500 mx-auto" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                <p class="mt-2 text-gray-600">Loading posts...</p>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-white mt-12 py-6 border-t">
        <div class="container mx-auto px-6 text-center text-gray-600">
            <p>&copy; 2025 Prabhas Holland. All rights reserved.</p>
        </div>
    </footer>

    <!-- About Me Modal -->
    <div id="about-modal" class="fixed inset-0 bg-black bg-opacity-50 z-20 hidden items-center justify-center transition-opacity duration-300">
        <div id="modal-content" class="bg-white p-8 rounded-lg shadow-xl max-w-md w-full m-4 transform scale-95 transition-transform duration-300">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-2xl font-bold">About Me</h2>
                <button id="close-modal-btn" class="text-3xl font-light leading-none text-gray-500 hover:text-gray-800">&times;</button>
            </div>
            <img src="https://placehold.co/400x200/3b82f6/ffffff?text=Welcome!" alt="Welcome Banner" class="rounded-lg mb-4">
            <p class="text-gray-700">
                Hi, I'm Prabhas Holland, an aspiring IT student currently based in <strong>Tirupati, Andhra Pradesh, India</strong>. I have a passion for web development, clean code, and exploring new technologies. This blog is my digital space where I share my learnings, projects, and thoughts on the ever-evolving world of tech.
            </p>
        </div>
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // --- Blog Post Data (simulating a JSON source) ---
            const posts = [
                {
                    title: "Getting Started with Modern Web Development",
                    author: "Prabhas Holland",
                    date: "September 30, 2025",
                    content: "Web development has evolved rapidly over the past decade. Today, building a modern website involves understanding a rich ecosystem of tools and frameworks. This post provides a high-level overview of the essential components: HTML, CSS, and JavaScript, and then dives into popular frameworks like React and Vue. We'll also touch upon the importance of responsive design and how to achieve it using CSS libraries like Tailwind CSS, which allows for rapid, utility-first styling. Finally, we discuss the role of version control with Git, a must-have skill for any developer looking to collaborate on projects."
                },
                {
                    title: "A Deep Dive into JavaScript Promises",
                    author: "Prabhas Holland",
                    date: "September 25, 2025",
                    content: "Asynchronous programming is at the heart of JavaScript, and Promises are a fundamental part of managing asynchronous operations. A Promise is an object representing the eventual completion (or failure) of an asynchronous operation and its resulting value. This article breaks down the concept of Promises, explaining the different states—pending, fulfilled, and rejected. We'll explore how to create Promises and consume them using `.then()`, `.catch()`, and `.finally()`. We'll also compare them with older callback patterns and introduce the more modern `async/await` syntax, which provides a cleaner and more readable way to work with asynchronous code."
                },
                {
                    title: "10 Tips for Writing Clean and Maintainable Code",
                    author: "Jane Smith",
                    date: "September 20, 2025",
                    content: "Writing code that works is one thing; writing code that is clean, readable, and maintainable is another. Clean code is crucial for long-term project success, especially in a team environment. This post outlines ten practical tips for improving your code quality. We'll cover topics such as meaningful variable names, the importance of small, single-responsibility functions, writing clear comments (and knowing when not to), and following consistent coding conventions. Adopting these habits will not only make your code easier for others to understand but will also make it easier for you to debug and extend in the future."
                },
                {
                    title: "The Art of Minimalist Design",
                    author: "Prabhas Holland",
                    date: "September 15, 2025",
                    content: "Minimalism in web design is not just about using fewer elements; it's about making every element count. The principle of 'less is more' guides designers to create clean, uncluttered interfaces that focus on the user's primary goal. This approach emphasizes typography, white space, and a deliberate color palette to create a visually appealing and highly functional user experience. In this post, we'll explore the core principles of minimalist design, look at some inspiring examples, and provide practical advice on how to apply these concepts to your own projects to create websites that are both beautiful and effective."
                }
            ];

            const postsContainer = document.getElementById('posts-container');
            const loadingIndicator = document.getElementById('loading');

            // --- Function to render posts ---
            function renderPosts() {
                // Clear the container first
                postsContainer.innerHTML = '';

                if (posts.length === 0) {
                    postsContainer.innerHTML = '<p class="text-center text-gray-500">No posts found.</p>';
                    return;
                }

                posts.forEach(post => {
                    const postElement = document.createElement('article');
                    postElement.className = 'bg-white p-6 md:p-8 rounded-lg shadow-md hover:shadow-xl transition-shadow duration-300';

                    // Truncate content for the preview
                    const isLongContent = post.content.length > 250;
                    
                    postElement.innerHTML = `
                        <h3 class="text-2xl font-bold mb-2 text-gray-900">${post.title}</h3>
                        <div class="text-sm text-gray-500 mb-4">
                            <span>By ${post.author}</span> &middot;
                            <span>${post.date}</span>
                        </div>
                        <div class="post-content text-gray-700 leading-relaxed">
                           <p>${post.content}</p>
                        </div>
                        ${isLongContent ? '<span class="read-more-btn font-semibold mt-4 inline-block">Read More</span>' : ''}
                    `;
                    postsContainer.appendChild(postElement);
                });
            }
            
            // --- Simulate a network delay then render ---
            setTimeout(() => {
                loadingIndicator.style.display = 'none';
                renderPosts();
            }, 1000);


            // --- Event delegation for "Read More" buttons ---
            postsContainer.addEventListener('click', function(e) {
                if (e.target && e.target.classList.contains('read-more-btn')) {
                    const contentDiv = e.target.previousElementSibling;
                    contentDiv.classList.toggle('expanded');
                    if (contentDiv.classList.contains('expanded')) {
                        e.target.textContent = 'Read Less';
                    } else {
                        e.target.textContent = 'Read More';
                    }
                }
            });

            // --- Modal Logic ---
            const aboutLink = document.getElementById('about-link');
            const aboutModal = document.getElementById('about-modal');
            const modalContent = document.getElementById('modal-content');
            const closeModalBtn = document.getElementById('close-modal-btn');

            const openModal = () => {
                aboutModal.classList.remove('hidden');
                aboutModal.classList.add('flex');
                setTimeout(() => {
                    aboutModal.classList.add('opacity-100');
                    modalContent.classList.remove('scale-95');
                }, 10); // Short delay for transition
            };

            const closeModal = () => {
                aboutModal.classList.remove('opacity-100');
                modalContent.classList.add('scale-95');
                setTimeout(() => {
                    aboutModal.classList.add('hidden');
                    aboutModal.classList.remove('flex');
                }, 300); // Match transition duration
            };

            aboutLink.addEventListener('click', (e) => {
                e.preventDefault();
                openModal();
            });

            closeModalBtn.addEventListener('click', closeModal);

            aboutModal.addEventListener('click', (e) => {
                if (e.target === aboutModal) {
                    closeModal();
                }
            });

            document.addEventListener('keydown', (e) => {
                if (e.key === "Escape" && !aboutModal.classList.contains('hidden')) {
                    closeModal();
                }
            });
        });
    </script>
</body>
</html>

