# Circle CI Video Recording

Circle CI Video Recording is an example configuration of Circle CI's config.yml and fastlane's Fastfile which lets you record your iOS XCTests. It uses applescript files for the recording process.


## Getting Started

You need to initialize Circle CI and fastlane in order to use Circle CI Video Recording. You can setup Circle CI by following [this documentation](https://circleci.com/docs/2.0/getting-started/) and fastlane with [this one](https://docs.fastlane.tools/getting-started/ios/setup/).

## Usage

Copy .circleci and Fastlane folders from Circle CI Video Recording to your project. After this, with every commit you make to Github, a commit workflow which runs XCTests will start on CircleCI and the recording will be saved as artifacts to CircleCI.

## How It Works
The commit triggers the `build_test` job on Circle CI, the job runs `build_and_test` lane from Fastlane.
The `build_and_test` job first builds the application, then starts video recording. After the recording is started, it runs the tests and stops recording at the end.

### Start Video Recording Applescript
```
tell application "QuickTime Player"
	set recording to new screen recording
	tell recording
		start
		delay 3
	end tell
end tell
```

### Stop Video Recording Applescript
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
## Author

[Connected2.me](http://connected2.me) / <a href="mailto:ddu@oriens.co">Dogu Deniz Ugur</a> <a href="https://github.com/DoguD">@DoguD</a>

## License

Circle CI Video Recording is available under the MIT license. See the LICENSE file for more info.
