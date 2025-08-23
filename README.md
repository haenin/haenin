
# (선택) 동영상에서 프레임 추출
# brew install ffmpeg
ffmpeg -i hello-kitty-walk.mp4 -vf "fps=12,scale=120:-1:flags=lanczos" frames/%03d.png

# 프레임 → GIF
# brew install imagemagick
magick convert -delay 8 -loop 0 frames/*.png -dispose previous -layers optimize assets/cat-walk.gif

# (선택) 용량 줄이기
# brew install gifsicle
gifsicle -O3 assets/cat-walk.gif -o assets/cat-walk.gif
