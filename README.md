#Building SubscriptionsCommon and OneApp

Fork the SubscriptionsCommon to your own repository

1. >git clone https://githubext.deere.com/YOUR_NAME/SubscriptionsCommon.git

    Change https to `ssh` above

2. >git remote add upstream https://githubext.deere.com/OBAMobileApps/SubscriptionsCommon.git

    Change https to `ssh` above

    Make sure you have your remote, and upstream repositories

   >git remote -v

3. >scripts/build_setup.sh

4. >rake clean_build_test

    If setup fails, run: `tail -f setup.log` to view the error

	  If error says opencv is not installed, run:	`brew tap homebrew/science`
	
5. >brew install opencv

	  If opencv is installed but build fails, follow the steps below:
	
		
		brew uninstall --force opencv
		rm /usr/local/bin/libpng-config
		brew link --overwrite libpng
		brew install opencv
		



6. >scripts/build_oneapp_calabash.sh
    
    If build hangs, try: `sudo gem install cocoapods`
	  and run: 
	  
	```
	rvm get head
	bundle install
	```
	
	If install hangs here trying to fetch baloo.git (can view running: tail -f setup.log) navigate to your `USER/.rvm/`
		
	>rm -rf gems/ruby-2.2.3/cache/bundler/git/baloo-829474b3489e4882027c7f66b198f728ee9c9114/
	
	>bundle install

	If error `linker command failed with exit code 1 (use -v to see invocation)` is given, run
	
	>open Subscriptions.xcworkspace/

7. >bundle exec cucumber features/oneapp.feature
	
	Depending on the status of the tests, this should pass. However, due to some bugs/flaky tests it may fail

	If output shows multiple errors: Failing Scenarios:
	```
	cucumber features/oneapp.feature:4 # Scenario: Downloading Subscriptions - Displaying Subscriptions from MyJD
	cucumber features/oneapp.feature:13 # Scenario: Manual Refresh of Subscriptions - MyJD available
  This test ensures the automatic download of subscriptions happens before
  tapping the refresh button so that we can confidently assert that 
  subscriptions were downloaded as a result of the refresh button being tapped
cucumber features/oneapp.feature:30 # Scenario: Persist subscriptions
cucumber features/oneapp.feature:41 # Scenario: Connect to WDS
cucumber features/oneapp.feature:54 # Scenario: Activate WDS
	```
  
  Run the following command in the `/OneApp` directory
  
  >rake calabash_build
  
  If presented with a pod update error,
	
	> pod install
	
	Then try step 7. again
