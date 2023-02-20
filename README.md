# How to use Cloudinary

<h4>Step 1: Create a cloudinary account on cloudinary.com</h4>

<h4>Step 2: Install cloudinary</h4>
 
{
  npm I cloudinary
}
 
<h4>Step 3: Create a utilities folder and then create a cloudinary.js file(optional, just for clean code)
Then in your cloudinary.js file type in this config. Your API key and secret would be gotten from the cloudinary account.</h4>
 
{
  const cloudinary = require("cloudinary").v2;
 
cloudinary.config({
    cloud_name: process.env.CLOUDNAME,
    api_key: process.env.API_KEY,
    api_secret: process.env.API_SECRET
});
 
module.exports = cloudinary;
 
}
 
<h4>Step 4: Then in your route where the image or video upload takes place you import the cloudinary variable</h4>
 
{
  const cloudinary = require("../utilities/cloudinary");
}
 
<h4>Step 5: Then in that same route you create an api for image or video upload</h4>
 
{<br>
 router.post("/imageupload", authenticateToken, async (req, res) => {
    const {image, name} = req.body;
 
  try{
 
    //uploads the image to cloudinary and gets the url and id
    const resultOfUpload = await cloudinary.uploader.upload(image, {folder:     "testUpload"}, (error, result)=>{
       console.log(result, error);
     });
 
    /* returns the url and id to the client side, this can also be saved in a PostgreSQL database or any other database */
     return res.status(200).json({
       id: resultOfUpload.public_id,
       url: resultOfUpload.secure_url
     })
 
  } catch (error){
    console.log(error.message);
    return res.status(401).json({ error: error.message });
  }
});
  <br>
}
