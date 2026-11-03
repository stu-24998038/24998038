# Capstone Portfolio & Blog - Backend API

Full-Stack Personal Portfolio Website backend built with Node.js, Express, and MongoDB.

## Live URLs

- **Frontend (Vercel):** https://capstone-portfolio-fmfr.vercel.app
- **Backend (Alibaba Cloud):** _(deploy after setup)_

## Tech Stack

- Node.js + Express
- MongoDB Atlas (Mongoose)
- JWT Authentication (bcryptjs)
- Helmet (security headers), CORS, dotenv

## Project Structure

```
capstone-backend/
├── config/db.js          # MongoDB connection
├── middleware/
│   ├── auth.js           # JWT protect middleware
│   └── errorMiddleware.js
├── models/
│   ├── User.js
│   ├── Project.js
│   ├── BlogPost.js
│   ├── Comment.js
│   └── Message.js
├── controllers/
│   ├── authController.js
│   ├── projectController.js
│   ├── blogController.js
│   ├── commentController.js
│   └── contactController.js
├── routes/
│   ├── users.js
│   ├── projects.js
│   ├── blog.js
│   └── contact.js
├── server.js
└── .env
```

## API Endpoints

### Auth
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | /api/users/register | Public | Register new user |
| POST | /api/users/login | Public | Login, returns JWT |

### Projects
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | /api/projects | Public | Get all projects |
| GET | /api/projects/:id | Public | Get single project |
| POST | /api/projects | Private | Create project |
| PUT | /api/projects/:id | Private (owner) | Update project |
| DELETE | /api/projects/:id | Private (owner) | Delete project |

### Blog
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | /api/blog | Public | Get all posts (populates author) |
| GET | /api/blog/:id | Public | Get single post (author + comments) |
| POST | /api/blog | Private | Create post |
| PUT | /api/blog/:id | Private (author) | Update post |
| DELETE | /api/blog/:id | Private (author) | Delete post |

### Comments
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | /api/blog/:postId/comments | Public | Get comments for a post |
| POST | /api/blog/:postId/comments | Private | Add a comment |

### Contact
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | /api/contact | Public | Submit contact form |

## Local Setup

```bash
npm install
npm run dev    # development with auto-reload
# or
npm start      # production
```

Make sure `.env` is configured:
```
PORT=5000
MONGO_URI=your_mongodb_atlas_uri
JWT_SECRET=your_jwt_secret
CORS_ORIGIN=https://your-frontend.vercel.app
```

## Deploy to Alibaba Cloud (ECS)

1. Connect to your ECS via SSH
2. Install Node.js 18+:
   ```bash
   curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
   sudo yum install -y nodejs
   ```
3. Upload this folder to `/var/www/capstone-backend`
4. Run `npm install --production`
5. Install PM2 to keep it running:
   ```bash
   sudo npm install -g pm2
   pm2 start server.js --name capstone-api
   pm2 startup
   pm2 save
   ```
6. Configure Nginx reverse proxy (optional) or open port 5000 in the security group
7. Update `.env`: set `NODE_ENV=production` and `CORS_ORIGIN` to your Vercel frontend URL
8. Update the frontend's API base URL to `http://your-aliyun-ip:5000`
