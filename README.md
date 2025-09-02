## 🎬 Movie App

A modern movie discovery app built with React and Vite that lets users explore trending movies, search for specific titles, and fetch movie details using The Movie Database (TMDB) API.
Additionally, the app integrates Appwrite as a database to store and check whether a searched movie exists in the database.

## 🚀 Features

🔍 Search Movies – Search for movies using keywords.

🎥 Trending & Popular Movies – Display trending and popular movies fetched from TMDB.

🗃️ Appwrite Integration – Stores search terms and checks if they exist in the database.

⚡ Fast & Optimized – Built using Vite for lightning-fast development and production builds.

📱 Responsive Design – Works seamlessly on desktop, tablet, and mobile.


🔧 #Installation & Setup

Follow these steps to run the project locally:

1️⃣ Clone the repository
git clone https://github.com/theshishuraj/react-movie-app.git

2️⃣ Navigate to the project folder
```bash
  cd movie-app
```

3️⃣ Install dependencies
```bash
npm install
```

4️⃣ Start the development server
```bash
npm run dev
```



🌐 ##API Integration

This project uses TMDB API to fetch movie data.
Sign up at https://www.themoviedb.org/
 and generate your API key.

Example API Request:

const fetchMovies = async (query = '') => {
    try {
      const endpoint = query 
      ? `${API_BASE_URL}/search/movie?query=${encodeURIComponent(query)}` 
      :`${API_BASE_URL}/discover/movie?sort_by=popularity.desc`;

      const response = await fetch(endpoint, API_OPTIONS);
}

##🗃️ Appwrite Integration

The app uses Appwrite's JavaScript SDK to check whether the searched movie term exists in the database.

Example Usage:


```javascript
import { Client, Databases, ID, Query } from "appwrite";

const PROJECT_ID = import.meta.env.VITE_APPWRITE_PROJECT_ID;
const DATABASE_ID = import.meta.env.VITE_APPWRITE_DATABASE_ID;
const COLLECTION_ID = import.meta.env.VITE_APPWRITE_COLLECTION_ID;

const client = new Client()
  .setEndpoint("https://fra.cloud.appwrite.io/v1")
  .setProject(PROJECT_ID);

const database = new Databases(client);

export const updateSearchCount = async (searchTerm, movie) => {
  //1. Use Appwrite SDK to check if the search term exists in the database
  try {
    const result = await database.listDocuments(DATABASE_ID, COLLECTION_ID, [
      Query.equal("searchTerm", searchTerm),
    ])

    //2. If it dose, update the count
    if (result.documents.length > 0) {
        const doc = result.documents[0];

        await database.updateDocument(DATABASE_ID, COLLECTION_ID, doc.$id, {
            count : doc.count + 1,
        })
    } 
    //3. If it doesn't, create a new document with the search term and count as 1
    else {
        await database.createDocument(DATABASE_ID, COLLECTION_ID, ID.unique(), {
            searchTerm,
            count : 1,
            movie_id : movie.id,
            poster_url: `https://image.tmdb.org/t/p/w500${movie.poster_path}`
        })
    }
  } catch (error) {
    console.log(error);
  }
};

export const getTrendingMovies = async () => {
    try {
        const result = await database.listDocuments(DATABASE_ID, COLLECTION_ID, [
            Query.limit(5),
            Query.orderDesc("count")
        ])

        return result.documents;
    } catch (error) {
        console.log(error);
        
    }
}
```





👨‍💻 #Author

Shishu Raj

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/shishu-raj-1536a118b/)
