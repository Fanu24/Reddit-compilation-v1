------------------------------------------------------------------------
                         Reddit Video Compilation
------------------------------------------------------------------------

This script allows you to download top-rated videos from a subreddit and create
a compilation of those videos. The downloaded videos are concatenated and saved
as a single video file.

------------------------------------------------------------------------
                            Prerequisites
------------------------------------------------------------------------

1. Python 3.x: Make sure you have Python 3.x installed on your system.

2. Required Python packages: Install the following packages using pip:

   - praw
   - youtube_dl
   - moviepy

      Example: pip install praw youtube_dl moviepy

3. Reddit API Credentials: Obtain Reddit API credentials by creating a Reddit
   developer application. You need the client ID, client secret, and user agent
   to access the Reddit API.

------------------------------------------------------------------------
                          How to Use the Script
------------------------------------------------------------------------

1. Open the script file in a Python editor or IDE.

2. Replace the placeholders 'YOUR_CLIENT_ID' and 'YOUR_CLIENT_SECRET' with
   your Reddit API credentials obtained in the prerequisites.

3. Specify the desired subreddit name in the 'subreddit_name' variable. This
   determines the subreddit from which the top-rated videos will be downloaded.

4. Set the 'limit' variable to the number of top-rated videos you want to
   download.

5. Run the script. It will download the top-rated videos from the specified
   subreddit and create a compilation of those videos.

6. The compilation will be saved as 'compilation.mp4' in the same directory
   as the script.

------------------------------------------------------------------------
                           Code Explanation
------------------------------------------------------------------------

The script is divided into several functions to perform specific tasks:

1. download_video(url, filename): This function uses the youtube_dl library
   to download a video from a given URL and save it with the specified filename.

2. create_compilation(video_files, output_file): This function takes a list of
   video files, concatenates them, resizes the clips, sets the total duration,
   and saves the compilation as a video file.

3. download_top_videos_from_subreddit(subreddit_name, limit): This function
   downloads the top-rated videos from the specified subreddit using the PRAW
   (Python Reddit API Wrapper) library. It filters out non-video posts and
   returns a list of downloaded video filenames.

The main part of the script performs the following steps:

1. Initialize the Reddit API using the praw.Reddit() constructor.

2. Call the download_top_videos_from_subreddit() function to download the
   top-rated videos from the specified subreddit. It returns a list of video
   filenames.

3. Call the create_compilation() function to create a compilation using the
   downloaded video files. It saves the compilation as 'compilation.mp4'.

------------------------------------------------------------------------
                           Additional Features
------------------------------------------------------------------------

1. Duration Calculation: The modified code includes the calculation of the
   compilation duration before proceeding with the compilation creation.
   The duration is displayed to the user.

2. User Confirmation: The script asks the user for confirmation before
   proceeding with the compilation creation. The user can choose to proceed
   or cancel the compilation creation.

------------------------------------------------------------------------
                              Error Handling
------------------------------------------------------------------------

The script includes error handling to catch exceptions during video download
and compilation creation. If an error occurs during video download, the script
prints an error message and continues with the next video. If an error occurs
during compilation creation, the script prints an error message and exits.

------------------------------------------------------------------------
