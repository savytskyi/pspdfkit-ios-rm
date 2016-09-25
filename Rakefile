# -*- coding: utf-8 -*-
$:.unshift("/Library/RubyMotion/lib")
require 'motion/project/template/ios'

begin
  require 'bundler'
  Bundler.require
rescue LoadError
end

Motion::Project::App.setup do |app|
  # Use `rake config' to see complete project settings.
  app.name = 'pspdfkit-ios10'

  # TODO: Change identifier:
  app.identifier = 'com.xxxxx.xxxxxxxx'
  app.embedded_frameworks << 'vendor/PSPDFKit.framework'

  # TODO: Change seed_id:
  app.seed_id = 'XXXXXXX'
  app.entitlements['get-task-allow'] = false
  app.entitlements['application-identifier'] = app.seed_id + '.' + app.identifier


  app.development do
    use_adhoc = true
    if use_adhoc
      # Ad Hoc Provisioing
      app.provisioning_profile  = "./certificates/adhoc.mobileprovision"
      # TODO: Change codesign_certificate:
      app.codesign_certificate  = 'iPhone Distribution: XXXXX (XXXXXX)'
      app.entitlements['get-task-allow'] = false
    else
      # # Dev Provisioning
      app.entitlements['get-task-allow'] = true
      app.provisioning_profile  = "./certificates/dev.mobileprovision"
      # TODO: Change codesign_certificate:
      app.codesign_certificate  = 'iPhone Developer: XXXXXX (XXXXXXXX)'
    end
  end

  app.release do
    app.entitlements['beta-reports-active'] = true

    # Setup Provisioning Profiles:
    app.provisioning_profile  = "./certificates/appstore.mobileprovision"
    # TODO: Change codesign_certificate:
    app.codesign_certificate  = 'iPhone Distribution: XXXXXXX (XXXXXXX)'
  end

end

desc 'Remove unwanted architecures for PSPDFKit'
task "build:device" do
  env = {
    EXPANDED_CODE_SIGN_IDENTITY_NAME: App.config.codesign_certificate,
    EXPANDED_CODE_SIGN_IDENTITY: App.config.codesign_certificate,
    VALID_ARCHS: App.config.archs['iPhoneOS'].join(','),
    BUILT_PRODUCTS_DIR: File.join(App.config.versionized_build_dir('iPhoneOS'), App.config.bundle_filename),
    FRAMEWORKS_FOLDER_PATH: 'Frameworks',
    CODE_SIGNING_REQUIRED: true,
    CONFIGURATION_BUILD_DIR: App.config.versionized_build_dir('iPhoneOS')
  }
  sh "env #{env.map { |k,v| "#{k}=\"#{v}\"" }.join(' ')} sh vendor/PSPDFKit.framework/strip-framework.sh"
end