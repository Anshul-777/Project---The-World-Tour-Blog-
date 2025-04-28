
// server.js
const express = require('express'),
      mongoose = require('mongoose'),
      multer   = require('multer'),
      cors     = require('cors'),
      app      = express();

// 1) Connect to MongoDB
mongoose.connect('mongodb://127.0.0.1:27017/blogApp', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// 2) Blog schema
const Blog = mongoose.model('Blog', new mongoose.Schema({
  title: String,
  content: String,
  imagePath: String,
  createdAt: { type: Date, default: Date.now }
}));

// 3) Middleware
app.use(cors());
app.use(express.json());
app.use('/uploads', express.static('uploads'));

// 4) Multer setup
const upload = multer({
  storage: multer.diskStorage({
    destination: (req, file, cb) => cb(null, 'uploads/'),
    filename:    (req, file, cb) => cb(null, Date.now() + '-' + file.originalname)
  })
});

// 5) Create blog endpoint
app.post('/api/blogs', upload.single('image'), async (req, res) => {
  const { title, content } = req.body;
  const imagePath = req.file ? req.file.path : '';
  const blog = await new Blog({ title, content, imagePath }).save();
  res.status(201).json(blog);
});

// 6) List blogs
app.get('/api/blogs', async (req, res) => {
  const blogs = await Blog.find().sort({ createdAt: -1 });
  res.json(blogs);
});

// 7) Start server
app.listen(5000, () => console.log('Server running on http://localhost:5000'));
