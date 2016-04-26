# AUTHOR: Charles Harroch

require 'yaml'
require 'ostruct'

fastlane_version "1.81.0"

config = YAML.load(open(File.join(File.dirname(__FILE__), "fastlane_config.yaml")))
settings = OpenStruct.new(config)

import "Fastfile_#{settings['platform']}"
