pricetag/
├── README.md
├── frontend/
│   ├── package.json
│   ├── pages/
│   │   └── index.js
│   ├── public/
│   │   └── placeholder.png
│   └── next.config.js
├── backend/
│   ├── package.json
│   ├── server.js
│   └── .env.example
└── .gitignore

{
  "name": "pricetag-frontend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "13.5.2",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  }
}

import { useState } from "react";

export default function Home() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [aiData, setAiData] = useState(null);

  const searchProducts = async () => {
    const res = await fetch(`http://localhost:5000/api/search?q=${query}`);
    const data = await res.json();
    setAiData(data.ai);
    setResults(data.results);
  };

  return (
    <div style={{ padding: "20px" }}>
      <h1>Pricetag</h1>
      <input
        placeholder="Search for any product..."
        onChange={(e) => setQuery(e.target.value)}
      />
      <button onClick={searchProducts}>Search</button>

      {aiData && (
        <div>
          <h3>🧠 AI Understanding:</h3>
          <pre>{aiData}</pre>
        </div>
      )}

      {results.map((item, i) => (
        <div key={i} style={{ margin: "20px 0" }}>
          <h2>{item.title}</h2>
          <img src={item.image} width="150" />
          {item.platforms.map((p, index) => (
            <div key={index}>
              {p.name} RM{p.price}{" "}
              <a href={p.link} target="_blank">
                View Deal →
              </a>
            </div>
          ))}
        </div>
      ))}
    </div>
  );
}

/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
};

module.exports = nextConfig;

{
  "name": "pricetag-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "openai": "^4.33.0"
  }
}




