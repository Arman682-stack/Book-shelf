# Book-shelf
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Library Management System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .book-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .book-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
        }
        .dashboard-stats {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <!-- Login/Register Modal -->
    <div id="authModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg p-8 w-96 max-w-md mx-4">
            <div class="text-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800" id="modalTitle">Login to Library</h2>
                <p class="text-gray-600">Access your account to manage books</p>
            </div>
            <form id="authForm">
                <div id="registerFields" class="hidden">
                    <div class="mb-4">
                        <label class="block text-gray-700 text-sm font-bold mb-2" for="fullName">Full Name</label>
                        <input type="text" id="fullName" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="email">Email</label>
                    <input type="email" id="email" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-6">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Password</label>
                    <input type="password" id="password" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <button type="submit" class="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <span id="submitText">Login</span>
                </button>
            </form>
            <div class="text-center mt-4">
                <button id="toggleAuth" class="text-blue-600 hover:text-blue-800 text-sm">
                    Don't have an account? Register here
                </button>
            </div>
        </div>
    </div>

    <!-- Add Book Modal -->
    <div id="addBookModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg p-8 w-96 max-w-md mx-4">
            <div class="text-center mb-6">
                <h2 class="text-2xl font-bold text-gray-800">Add New Book</h2>
            </div>
            <form id="addBookForm">
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bookTitle">Title</label>
                    <input type="text" id="bookTitle" required class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bookAuthor">Author</label>
                    <input type="text" id="bookAuthor" required class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bookIsbn">ISBN</label>
                    <input type="text" id="bookIsbn" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bookCategory">Category</label>
                    <select id="bookCategory" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="Fiction">Fiction</option>
                        <option value="Non-Fiction">Non-Fiction</option>
                        <option value="Science">Science</option>
                        <option value="Technology">Technology</option>
                        <option value="History">History</option>
                        <option value="Biography">Biography</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bookCopies">Number of Copies</label>
                    <input type="number" id="bookCopies" min="1" value="1" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div class="flex space-x-4">
                    <button type="submit" class="flex-1 bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">Add Book</button>
                    <button type="button" id="cancelAddBook" class="flex-1 bg-gray-300 text-gray-700 py-2 px-4 rounded-md hover:bg-gray-400">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="bg-white shadow-lg">
        <div class="max-w-7xl mx-auto px-4">
            <div class="flex justify-between items-center py-4">
                <div class="flex items-center">
                    <div class="w-10 h-10 rounded-full bg-blue-600 flex items-center justify-center text-white font-bold mr-3">
                        <i class="fas fa-book"></i>
                    </div>
                    <span class="text-xl font-bold text-gray-800">Library System</span>
                </div>
                
                <div class="hidden md:flex items-center space-6">
                    <a href="#dashboard" class="text-gray-600 hover:text-blue-600 px-3 py-2">Dashboard</a>
                    <a href="#books" class="text-gray-600 hover:text-blue-600 px-3 py-2">Books</a>
                    <div class="flex items-center space-x-4 ml-6">
                        <span id="userWelcome" class="text-gray-700 hidden"></span>
                        <button id="loginBtn" class="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700">Login</button>
                        <button id="logoutBtn" class="bg-red-600 text-white px-4 py-2 rounded-md hover:bg-red-700 hidden">Logout</button>
                    </div>
                </div>
                
                <button id="mobileMenuBtn" class="md:hidden text-gray-600">
                    <i class="fas fa-bars text-2xl"></i>
                </button>
            </div>
        </div>
    </nav>

    <!-- Mobile Menu -->
    <div id="mobileMenu" class="fixed inset-0 bg-white z-40 hidden md:hidden">
        <div class="p-6">
            <div class="flex justify-between items-center mb-8">
                <span class="text-xl font-bold text-gray-800">Menu</span>
                <button id="closeMobileMenu" class="text-gray-600">
                    <i class="fas fa-times text-2xl"></i>
                </button>
            </div>
            <div class="space-y-4">
                <a href="#dashboard" class="block py-3 text-gray-600 hover:text-blue-600 border-b">Dashboard</a>
                <a href="#books" class="block py-3 text-gray-600 hover:text-blue-600 border-b">Books</a>
                <div class="pt-4">
                    <button id="mobileLoginBtn" class="w-full bg-blue-600 text-white py-3 rounded-md hover:bg-blue-700 mb-2">Login</button>
                    <button id="mobileLogoutBtn" class="w-full bg-red-600 text-white py-3 rounded-md hover:bg-red-700 hidden">Logout</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Hero Section -->
    <section class="gradient-bg text-white py-20">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <h1 class="text-4xl md:text-6xl font-bold mb-6">Digital Library Management</h1>
            <p class="text-xl md:text-2xl mb-8">Discover, borrow, and manage books with our modern library system</p>
            <div class="flex flex-col sm:flex-row justify-center space-y-4 sm:space-y-0 sm:space-x-4">
                <button id="exploreBooks" class="bg-white text-blue-600 px-8 py-3 rounded-md font-semibold hover:bg-gray-100">Explore Books</button>
                <button id="getStarted" class="border-2 border-white text-white px-8 py-3 rounded-md font-semibold hover:bg-white hover:text-blue-600">Get Started</button>
            </div>
        </div>
    </section>

    <!-- Search Section -->
    <section class="bg-white py-12">
        <div class="max-w-7xl mx-auto px-4">
            <div class="bg-gray-100 rounded-lg p-6 shadow-sm">
                <div class="flex flex-col md:flex-row gap-4">
                    <input type="text" id="searchInput" placeholder="Search by title, author, or category..." class="flex-1 px-4 py-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <select id="categoryFilter" class="px-4 py-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="">All Categories</option>
                        <option value="Fiction">Fiction</option>
                        <option value="Non-Fiction">Non-Fiction</option>
                        <option value="Science">Science</option>
                        <option value="Technology">Technology</option>
                        <option value="History">History</option>
                        <option value="Biography">Biography</option>
                    </select>
                    <button id="searchBtn" class="bg-blue-600 text-white px-6 py-3 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <i class="fas fa-search mr-2"></i>Search
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Books Section -->
    <section id="books" class="py-16 bg-gray-50">
        <div class="max-w-7xl mx-auto px-4">
            <div class="flex justify-between items-center mb-8">
                <h2 class="text-3xl font-bold text-gray-800">Library Collection</h2>
                <button id="addBookBtn" class="bg-green-600 text-white px-4 py-2 rounded-md hover:bg-green-700 hidden">
                    <i class="fas fa-plus mr-2"></i>Add Book
                </button>
            </div>
            
            <div id="booksContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                <!-- Books will be dynamically loaded here -->
            </div>
            
            <div id="noBooksMessage" class="text-center py-12 hidden">
                <div class="w-24 h-24 mx-auto mb-6 bg-gray-200 rounded-full flex items-center justify-center">
                    <i class="fas fa-book-open text-4xl text-gray-400"></i>
                </div>
                <h3 class="text-xl font-semibold text-gray-600 mb-2">No books found</h3>
                <p class="text-gray-500">Try adjusting your search criteria or add new books to the library.</p>
            </div>
        </div>
    </section>

    <!-- Dashboard Section -->
    <section id="dashboard" class="py-16 bg-white hidden">
        <div class="max-w-7xl mx-auto px-4">
            <h2 class="text-3xl font-bold text-gray-800 mb-8">My Dashboard</h2>
            
            <!-- Stats Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="dashboard-stats text-white p-6 rounded-lg">
                    <div class="flex items-center">
                        <div class="p-3 bg-white bg-opacity-20 rounded-full mr-4">
                            <i class="fas fa-book-open text-2xl"></i>
                        </div>
                        <div>
                            <p class="text-sm opacity-80">Total Books</p>
                            <p class="text-2xl font-bold" id="totalBooks">0</p>
                        </div>
                    </div>
                </div>
                
                <div class="dashboard-stats text-white p-6 rounded-lg">
                    <div class="flex items-center">
                        <div class="p-3 bg-white bg-opacity-20 rounded-full mr-4">
                            <i class="fas fa-user-check text-2xl"></i>
                        </div>
                        <div>
                            <p class="text-sm opacity-80">Books Borrowed</p>
                            <p class="text-2xl font-bold" id="borrowedBooks">0</p>
                        </div>
                    </div>
                </div>
                
                <div class="dashboard-stats text-white p-6 rounded-lg">
                    <div class="flex items-center">
                        <div class="p-3 bg-white bg-opacity-20 rounded-full mr-4">
                            <i class="fas fa-clock text-2xl"></i>
                        </div>
                        <div>
                            <p class="text-sm opacity-80">Due Soon</p>
                            <p class="text-2xl font-bold" id="dueSoon">0</p>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Borrowed Books -->
            <div class="bg-gray-50 rounded-lg p-6">
                <h3 class="text-xl font-semibold text-gray-800 mb-6">My Borrowed Books</h3>
                <div id="borrowedBooksList" class="space-y-4">
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-inbox text-4xl mb-4"></i>
                        <p>You haven't borrowed any books yet.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-12">
        <div class="max-w-7xl mx-auto px-4">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div>
                    <h3 class="text-xl font-bold mb-4">Library System</h3>
                    <p class="text-gray-400">Modern digital library management solution for schools, universities, and public libraries.</p>
                </div>
                <div>
                    <h3 class="text-xl font-bold mb-4">Quick Links</h3>
                    <ul class="space-y-2">
                        <li><a href="#dashboard" class="text-gray-400 hover:text-white">Dashboard</a></li>
                        <li><a href="#books" class="text-gray-400 hover:text-white">Books</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white">About</a></li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-xl font-bold mb-4">Contact</h3>
                    <p class="text-gray-400">Email: info@library.com</p>
                    <p class="text-gray-400">Phone: (555) 123-4567</p>
                </div>
            </div>
            <div class="border-t border-gray-700 mt-8 pt-8 text-center text-gray-400">
                <p>© 2024 Library Management System. All rights reserved.</p>
            </div>
        </div>
    </footer>

    <script>
        // Sample data initialization
        const initializeData = () => {
            if (!localStorage.getItem('books')) {
                const sampleBooks = [
                    {
                        id: 1,
                        title: 'The Great Gatsby',
                        author: 'F. Scott Fitzgerald',
                        isbn: '978-0-7432-7356-5',
                        category: 'Fiction',
                        copies: 5,
                        available: 5
                    },
                    {
                        id: 2,
                        title: 'To Kill a Mockingbird',
                        author: 'Harper Lee',
                        isbn: '978-0-06-112008-4',
                        category: 'Fiction',
                        copies: 3,
                        available: 3
                    },
                    {
                        id: 3,
                        title: '1984',
                        author: 'George Orwell',
                        isbn: '978-0-452-28423-4',
                        category: 'Fiction',
                        copies: 4,
                        available: 4
                    },
                    {
                        id: 4,
                        title: 'A Brief History of Time',
                        author: 'Stephen Hawking',
                        isbn: '978-0-553-10953-5',
                        category: 'Science',
                        copies: 2,
                        available: 2
                    },
                    {
                        id: 5,
                        title: 'Sapiens: A Brief History of Humankind',
                        author: 'Yuval Noah Harari',
                        isbn: '978-0-06-231609-7',
                        category: 'History',
                        copies: 3,
                        available: 3
                    }
                ];
                localStorage.setItem('books', JSON.stringify(sampleBooks));
            }
            
            if (!localStorage.getItem('users')) {
                localStorage.setItem('users', JSON.stringify([]));
            }
            
            if (!localStorage.getItem('transactions')) {
                localStorage.setItem('transactions', JSON.stringify([]));
            }
        };

        // Initialize application
        initializeData();

        // DOM Elements
        const authModal = document.getElementById('authModal');
        const modalTitle = document.getElementById('modalTitle');
        const submitText = document.getElementById('submitText');
        const registerFields = document.getElementById('registerFields');
        const authForm = document.getElementById('authForm');
        const toggleAuth = document.getElementById('toggleAuth');
        const loginBtn = document.getElementById('loginBtn');
        const logoutBtn = document.getElementById('logoutBtn');
        const mobileLoginBtn = document.getElementById('mobileLoginBtn');
        const mobileLogoutBtn = document.getElementById('mobileLogoutBtn');
        const userWelcome = document.getElementById('userWelcome');
        const addBookModal = document.getElementById('addBookModal');
        const addBookForm = document.getElementById('addBookForm');
        const addBookBtn = document.getElementById('addBookBtn');
        const cancelAddBook = document.getElementById('cancelAddBook');
        const booksContainer = document.getElementById('booksContainer');
        const noBooksMessage = document.getElementById('noBooksMessage');
        const searchInput = document.getElementById('searchInput');
        const categoryFilter = document.getElementById('categoryFilter');
        const searchBtn = document.getElementById('searchBtn');
        const dashboardSection = document.getElementById('dashboard');
        const totalBooks = document.getElementById('totalBooks');
        const borrowedBooks = document.getElementById('borrowedBooks');
        const dueSoon = document.getElementById('dueSoon');
        const borrowedBooksList = document.getElementById('borrowedBooksList');
        const mobileMenu = document.getElementById('mobileMenu');
        const mobileMenuBtn = document.getElementById('mobileMenuBtn');
        const closeMobileMenu = document.getElementById('closeMobileMenu');
        const exploreBooks = document.getElementById('exploreBooks');
        const getStarted = document.getElementById('getStarted');

        // State
        let isRegistering = false;
        let currentUser = null;
        let allBooks = JSON.parse(localStorage.getItem('books') || '[]');

        // Functions
        const showAuthModal = (isRegister = false) => {
            isRegistering = isRegister;
            modalTitle.textContent = isRegister ? 'Create Account' : 'Login to Library';
            submitText.textContent = isRegister ? 'Register' : 'Login';
            registerFields.classList.toggle('hidden', !isRegister);
            toggleAuth.textContent = isRegister ? 'Already have an account? Login here' : 'Don\'t have an account? Register here';
            authModal.classList.remove('hidden');
        };

        const hideAuthModal = () => {
            authModal.classList.add('hidden');
            authForm.reset();
        };

        const showAddBookModal = () => {
            addBookModal.classList.remove('hidden');
        };

        const hideAddBookModal = () => {
            addBookModal.classList.add('hidden');
            addBookForm.reset();
        };

        const registerUser = (userData) => {
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const userExists = users.find(user => user.email === userData.email);
            
            if (userExists) {
                alert('User with this email already exists');
                return false;
            }
            
            const newUser = {
                id: Date.now(),
                ...userData,
                isAdmin: users.length === 0 // First user is admin
            };
            
            users.push(newUser);
            localStorage.setItem('users', JSON.stringify(users));
            return newUser;
        };

        const loginUser = (email, password) => {
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const user = users.find(user => user.email === email && user.password === password);
            
            if (!user) {
                alert('Invalid email or password');
                return false;
            }
            
            return user;
        };

        const updateUI = () => {
            const isLoggedIn = !!currentUser;
            
            loginBtn.classList.toggle('hidden', isLoggedIn);
            logoutBtn.classList.toggle('hidden', !isLoggedIn);
            mobileLoginBtn.classList.toggle('hidden', isLoggedIn);
            mobileLogoutBtn.classList.toggle('hidden', !isLoggedIn);
            addBookBtn.classList.toggle('hidden', !(isLoggedIn && currentUser.isAdmin));
            dashboardSection.classList.toggle('hidden', !isLoggedIn);
            
            if (isLoggedIn) {
                userWelcome.textContent = `Welcome, ${currentUser.fullName || currentUser.email}`;
                userWelcome.classList.remove('hidden');
                updateDashboard();
            } else {
                userWelcome.classList.add('hidden');
            }
            
            loadBooks();
        };

        const loadBooks = (searchTerm = '', category = '') => {
            let filteredBooks = allBooks;
            
            if (searchTerm) {
                filteredBooks = filteredBooks.filter(book => 
                    book.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
                    book.author.toLowerCase().includes(searchTerm.toLowerCase())
                );
            }
            
            if (category) {
                filteredBooks = filteredBooks.filter(book => book.category === category);
            }
            
            booksContainer.innerHTML = '';
            
            if (filteredBooks.length === 0) {
                noBooksMessage.classList.remove('hidden');
                return;
            }
            
            noBooksMessage.classList.add('hidden');
            
            filteredBooks.forEach(book => {
                const bookCard = document.createElement('div');
                bookCard.className = 'book-card bg-white rounded-lg overflow-hidden shadow-md';
                
                bookCard.innerHTML = `
                    <div class="h-48 bg-gray-200 flex items-center justify-center">
                        <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/71961594-3e41-45da-a89f-0356e08b0680.png" alt="${book.title} book cover" class="w-full h-full object-cover" onerror="this.src='https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/159f3793-1b6f-43cd-aeda-509414256f91.png'">
                    </div>
                    <div class="p-6">
                        <h3 class="font-bold text-xl mb-2 text-gray-800">${book.title}</h3>
                        <p class="text-gray-600 mb-2">By ${book.author}</p>
                        <p class="text-sm text-gray-500 mb-4">${book.category} • ISBN: ${book.isbn}</p>
                        <div class="flex justify-between items-center">
                            <span class="text-sm ${book.available > 0 ? 'text-green-600' : 'text-red-600'}">
                                ${book.available > 0 ? `${book.available} available` : 'Out of stock'}
                            </span>
                            <button class="borrow-btn bg-blue-600 text-white px-4 py-2 rounded text-sm hover:bg-blue-700 ${book.available === 0 || !currentUser ? 'opacity-50 cursor-not-allowed' : ''}" 
                                ${book.available === 0 || !currentUser ? 'disabled' : ''} data-book-id="${book.id}">
                                Borrow
                            </button>
                        </div>
                    </div>
                `;
                
                booksContainer.appendChild(bookCard);
            });
            
            // Add event listeners to borrow buttons
            document.querySelectorAll('.borrow-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const bookId = parseInt(e.target.dataset.bookId);
                    borrowBook(bookId);
                });
            });
        };

        const borrowBook = (bookId) => {
            const book = allBooks.find(b => b.id === bookId);
            
            if (!book || book.available === 0) {
                alert('This book is not available for borrowing');
                return;
            }
            
            const transactions = JSON.parse(localStorage.getItem('transactions') || '[]');
            const dueDate = new Date();
            dueDate.setDate(dueDate.getDate() + 14); // 2 weeks from now
            
            const transaction = {
                id: Date.now(),
                bookId: book.id,
                userId: currentUser.id,
                bookTitle: book.title,
                borrowDate: new Date().toISOString(),
                dueDate: dueDate.toISOString(),
                returned: false
            };
            
            transactions.push(transaction);
            localStorage.setItem('transactions', JSON.stringify(transactions));
            
            // Update book availability
            book.available--;
            localStorage.setItem('books', JSON.stringify(allBooks));
            
            alert(`You have successfully borrowed "${book.title}". Due date: ${dueDate.toLocaleDateString()}`);
            updateUI();
        };

        const updateDashboard = () => {
            if (!currentUser) return;
            
            // Update stats
            totalBooks.textContent = allBooks.length;
            
            const transactions = JSON.parse(localStorage.getItem('transactions') || '[]');
            const userTransactions = transactions.filter(t => t.userId === currentUser.id && !t.returned);
            
            borrowedBooks.textContent = userTransactions.length;
            
            // Calculate due soon (within 3 days)
            const now = new Date();
            const dueSoonCount = userTransactions.filter(t => {
                const dueDate = new Date(t.dueDate);
                const diffTime = dueDate - now;
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                return diffDays <= 3 && diffDays > 0;
            }).length;
            
            dueSoon.textContent = dueSoonCount;
            
            // Update borrowed books list
            borrowedBooksList.innerHTML = '';
            
            if (userTransactions.length === 0) {
                borrowedBooksList.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-inbox text-4xl mb-4"></i>
                        <p>You haven't borrowed any books yet.</p>
                    </div>
                `;
                return;
            }
            
            userTransactions.forEach(transaction => {
                const dueDate = new Date(transaction.dueDate);
                const now = new Date();
                const diffTime = dueDate - now;
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                
                const transactionEl = document.createElement('div');
                transactionEl.className = 'bg-white p-4 rounded-lg border border-gray-200';
                
                transactionEl.innerHTML = `
                    <div class="flex justify-between items-start">
                        <div>
                            <h4 class="font-semibold text-gray-800">${transaction.bookTitle}</h4>
                            <p class="text-sm text-gray-600">Borrowed: ${new Date(transaction.borrowDate).toLocaleDateString()}</p>
                            <p class="text-sm ${diffDays <= 3 ? 'text-red-600' : 'text-gray-600'}">
                                Due: ${dueDate.toLocaleDateString()} 
                                ${diffDays <= 3 ? `(${diffDays} day${diffDays !== 1 ? 's' : ''} left)` : ''}
                            </p>
                        </div>
                        <button class="return-btn bg-green-600 text-white px-3 py-1 rounded text-sm hover:bg-green-700" data-transaction-id="${transaction.id}">
                            Return
                        </button>
                    </div>
                `;
                
                borrowedBooksList.appendChild(transactionEl);
            });
            
            // Add event listeners to return buttons
            document.querySelectorAll('.return-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const transactionId = parseInt(e.target.dataset.transactionId);
                    returnBook(transactionId);
                });
            });
        };

        const returnBook = (transactionId) => {
            const transactions = JSON.parse(localStorage.getItem('transactions') || '[]');
            const transaction = transactions.find(t => t.id === transactionId);
            
            if (!transaction) return;
            
            // Mark as returned
            transaction.returned = true;
            transaction.returnDate = new Date().toISOString();
            localStorage.setItem('transactions', JSON.stringify(transactions));
            
            // Update book availability
            const book = allBooks.find(b => b.id === transaction.bookId);
            if (book) {
                book.available++;
                localStorage.setItem('books', JSON.stringify(allBooks));
            }
            
            alert(`You have returned "${transaction.bookTitle}"`);
            updateUI();
        };

        const addBook = (bookData) => {
            const newBook = {
                id: Date.now(),
                ...bookData,
                available: parseInt(bookData.copies)
            };
            
            allBooks.push(newBook);
            localStorage.setItem('books', JSON.stringify(allBooks));
            
            alert(`Book "${bookData.title}" added successfully`);
            hideAddBookModal();
            updateUI();
        };

        // Event Listeners
        authForm.addEventListener('submit', (e) => {
            e.preventDefault();
            
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            if (isRegistering) {
                const fullName = document.getElementById('fullName').value;
                
                if (!fullName || !email || !password) {
                    alert('Please fill all fields');
                    return;
                }
                
                const newUser = registerUser({ fullName, email, password });
                if (newUser) {
                    currentUser = newUser;
                    localStorage.setItem('currentUser', JSON.stringify(currentUser));
                    hideAuthModal();
                    updateUI();
                }
            } else {
                if (!email || !password) {
                    alert('Please fill all fields');
                    return;
                }
                
                const user = loginUser(email, password);
                if (user) {
                    currentUser = user;
                    localStorage.setItem('currentUser', JSON.stringify(currentUser));
                    hideAuthModal();
                    updateUI();
                }
            }
        });

        toggleAuth.addEventListener('click', () => {
            showAuthModal(!isRegistering);
        });

        loginBtn.addEventListener('click', () => showAuthModal(false));
        mobileLoginBtn.addEventListener('click', () => showAuthModal(false));

        logoutBtn.addEventListener('click', () => {
            currentUser = null;
            localStorage.removeItem('currentUser');
            updateUI();
        });

        mobileLogoutBtn.addEventListener('click', () => {
            currentUser = null;
            localStorage.removeItem('currentUser');
            updateUI();
            mobileMenu.classList.add('hidden');
        });

        addBookBtn.addEventListener('click', showAddBookModal);
        cancelAddBook.addEventListener('click', hideAddBookModal);

        addBookForm.addEventListener('submit', (e) => {
            e.preventDefault();
            
            const bookData = {
                title: document.getElementById('bookTitle').value,
                author: document.getElementById('bookAuthor').value,
                isbn: document.getElementById('bookIsbn').value,
                category: document.getElementById('bookCategory').value,
                copies: document.getElementById('bookCopies').value
            };
            
            addBook(bookData);
        });

        searchBtn.addEventListener('click', () => {
            const searchTerm = searchInput.value;
            const category = categoryFilter.value;
            loadBooks(searchTerm, category);
        });

        searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                const searchTerm = searchInput.value;
                const category = categoryFilter.value;
                loadBooks(searchTerm, category);
            }
        });

        categoryFilter.addEventListener('change', () => {
            const searchTerm = searchInput.value;
            const category = categoryFilter.value;
            loadBooks(searchTerm, category);
        });

        mobileMenuBtn.addEventListener('click', () => {
            mobileMenu.classList.remove('hidden');
        });

        closeMobileMenu.addEventListener('click', () => {
            mobileMenu.classList.add('hidden');
        });

        exploreBooks.addEventListener('click', () => {
            document.querySelector('a[href="#books"]').click();
        });

        getStarted.addEventListener('click', () => {
            showAuthModal(true);
        });

        // Smooth scrolling for anchor links
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({
                        behavior: 'smooth'
                    });
                }
            });
        });

        // Initialize the app
        const savedUser = localStorage.getItem('currentUser');
        if (savedUser) {
            currentUser = JSON.parse(savedUser);
        }
        
        updateUI();
    </script>
</body>
</html>

