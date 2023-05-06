import praw
import youtube_dl
import moviepy.editor as mp
import subprocess

# PRAW Configuration (Reddit API)
reddit = praw.Reddit(client_id='YOUR_CLIENT_ID',
                     client_secret='YOUR_CLIENT_SECRET',
                     user_agent='Compilation by u/Fanu24')

# Function to download a video from Reddit using youtube_dl
def download_video(url, filename):
    ydl_opts = {'outtmpl': filename}
    with youtube_dl.YoutubeDL(ydl_opts) as ydl:
        ydl.download([url])

# Function to create a video compilation
def create_compilation(video_files):
    if len(video_files) == 0:
        print("No videos downloaded. Cannot create the compilation.")
        return None

    clips = []
    durations = []
    for file in video_files:
        try:
            clip = mp.VideoFileClip(file)
            clip_resized = clip.resize(height=720)
            clips.append(clip_resized)
            durations.append(clip_resized.duration)
        except OSError:
            print(f"Error reading video {file}. Skipping...")

    if len(clips) == 0:
        print("No valid videos found. Cannot create the compilation.")
        return None

    # Calculate the total duration of the compilation
    total_duration = sum(durations)

    # Concatenate the videos in the compilation
    final_clip = mp.concatenate_videoclips(clips, method="compose")

    # Set the duration of the compilation
    final_clip = final_clip.set_duration(total_duration)

    return final_clip

# Download videos from a specific subreddit
def download_videos_from_subreddit(subreddit_name, limit):
    subreddit = reddit.subreddit(subreddit_name)
    posts = subreddit.top(time_filter='all', limit=limit)

    video_files = []
    total_duration = 0
    for post in posts:
        if post.is_video:
            video_url = post.media['reddit_video']['fallback_url']
            video_filename = f"{post.id}.mp4"
            try:
                download_video(video_url, video_filename)
                video_files.append(video_filename)
                info_dict = subprocess.run(['ffprobe', '-v', 'error', '-select_streams', 'v:0', '-show_entries', 'stream=duration', '-of', 'default=noprint_wrappers=1:nokey=1', video_filename], capture_output=True, text=True)
                duration = float(info_dict.stdout)
                total_duration += duration
                print(f"Video {post.id} downloaded. Duration: {duration} seconds.")
            except Exception as e:
                print(f"Error downloading video {post.id}: {str(e)}")

    print(f"Total duration of downloaded videos: {total_duration} seconds.")
    return video_files

# Example usage
subreddit_name = 'subreddit_name'  # Specify the subreddit name to download videos from
limit = 10  # Set the limit of videos to download
video_files = download_videos_from_subreddit(subreddit_name, limit)
final_clip = create_compilation(video_files)
if final_clip is not None:
    output_file = 'compilation.mp4'

    # Calculate the duration of the compilation
    compilation_duration = final_clip.duration

    # Display the duration of the compilation
    print(f"Duration of the compilation: {compilation_duration} seconds.")

# Ask the user if they want to proceed with the compilation creation
proceed = input("Do you want to proceed with creating the compilation? (Yes/No): ")
if proceed.lower() == "yes":
    # Save the compilation to the output file
    final_clip.write_videofile(output_file, codec='libx264', audio_codec='aac')
    print(f"The compilation has been successfully created: {output_file}")
else:
    print("Compilation creation canceled.")
