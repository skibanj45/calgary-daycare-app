const express = require("express")
const mongoose = require("mongoose")
const cors = require("cors")
require("dotenv").config()

const daycareRoutes = require("./routes/daycares")

const app = express()
const PORT = process.env.PORT || 5000

// Middleware
app.use(cors())
app.use(express.json())

// Connect to MongoDB
mongoose.connect(process.env.MONGODB_URI || "mongodb://localhost:27017/calgary-daycares", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})

mongoose.connection.on("connected", () => {
  console.log("Connected to MongoDB")
})

mongoose.connection.on("error", (err) => {
  console.error("MongoDB connection error:", err)
})

// Routes
app.use("/api/daycares", daycareRoutes)

// Health check
app.get("/api/health", (req, res) => {
  res.json({ status: "OK", message: "Calgary Daycare Reviews API is running" })
})

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
