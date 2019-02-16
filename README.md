# CircleCI Video Recording

CircleCI Video Recording is an example configuration of CircleCI's config.yml and fastlane's Fastfile which lets you record your iOS XCTests. It uses applescript files for the recording process.


## Getting Started

You need to initialize CircleCI and fastlane in order to use CircleCI Video Recording. You can setup CircleCI by following [this documentation](https://circleci.com/docs/2.0/getting-started/) and fastlane with [this one](https://docs.fastlane.tools/getting-started/ios/setup/). MacOS environment is needed in order to run XCTests and record video of them. 

## Usage

Copy .circleci and Fastlane folders from CircleCI Video Recording to your project. After this, with every commit you make to Github, a commit workflow which runs XCTests will start on CircleCI and the recording will be saved as artifacts to CircleCI.

## How It Works
The commit triggers the `build_test` job on CircleCI, the job runs `build_and_test` lane from Fastlane.
The `build_and_test` job first builds the application, then starts video recording. After the recording is started, it runs the tests and stops recording at the end.

### Applescript which Starts Video Recording
```
tell application "QuickTime Player"
	set recording to new screen recording
	tell recording
		start
		delay 3
	end tell
end tell
```

### Applescript which Stops Video Recording
```
tell application "QuickTime Player"
	set recording to document "screen recording"
	tell recording
		pause
		set current_path to do shell script "pwd"
		set file_path to current_path & "/testrecording.mov"
		save in (file_path as POSIX file)
		-- export in ("../testrecording.mov" as POSIX file) using settings preset "480p"
		stop
		return "Test recording saved to " & file_path
		close
	end tell
end tell
```

## Formating Recorded Video

Applescript uses Quick Time and by default it saves videos as .mov files. However, these files can't be played on web and their file size is quite large. As a work around you can uncomment the below lines in Fastfile in order to convert the output video to .mp4 format.
```
sh("HOMEBREW_NO_AUTO_UPDATE=1 brew install ffmpeg")
sh('ffmpeg -y -i /Users/distiller/src/testrecording.mov -vf "setpts=1.25*PTS" -r 20 /Users/distiller/src/testrecording.mp4')
sh("rm /Users/distiller/src/testrecording.mov")
```

## Author

[Connected2.me](http://connected2.me) / <a href="mailto:ddu@oriens.co">Dogu Deniz Ugur</a> <a href="https://github.com/DoguD">@DoguD</a>

## License

CircleCI Video Recording is available under the MIT license. See the LICENSE file for more info.
