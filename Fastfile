default_platform(:ios)

platform :ios do

  desc "Archive and upload to TestFlight"
  lane :upload do
    # add actions here: https://docs.fastlane.tools/actions
    api_key = app_store_connect_api_key(
      key_id: "#{ENV['APP_STORE_API_KEY_ID']}",
      issuer_id: "#{ENV['APP_STORE_API_KEY_ISSUER_ID']}",
      key_filepath: "./fastlane/AuthKey_#{ENV['APP_STORE_API_KEY_ID']}.p8",
      duration: 500,
      in_house: false
    )
    previous_build_number = latest_testflight_build_number(
      app_identifier: "#{ENV['FASTLANE_APP_IDENTIFIER']}",
      api_key: api_key
    )
    current_build_number = previous_build_number + 1
    increment_build_number(
      xcodeproj: "./#{ENV['XCODEPROJ']}",
      build_number: current_build_number
    )
    gym(
    scheme: "#{ENV['GYM_SCHEME']}",
    output_directory: "#{ENV['GYM_OUTPUT_DIR']}",
    output_name: "#{ENV['GYM_OUTPUT_IPA_NAME']}",
    clean: true
    )
    pilot(
      ipa: "./#{ENV['GYM_OUTPUT_DIR']}/#{ENV['GYM_OUTPUT_IPA_NAME']}",
      skip_submission: true,
      skip_waiting_for_build_processing: true
    )
  end

  desc "Run all the unit tests"
  lane :test do
    scan(
      scheme: "#{ENV['TEST_SCHEME']}",
      workspace: "#{ENV['WORKSPACE']}",
      devices: "#{ENV['TEST_DEVICES']}"
    )
  end
  
  desc "Test and upload to Test Flight"
  lane :test_flight do
    test
    upload
  end

end
