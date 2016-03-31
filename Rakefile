#!/usr/bin/env rake
# frozen_string_literal: true
#
# Copyright (C) 2016 Harald Sitter <sitter@kde.org>
#
# This library is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 2.1 of the License, or (at your option) version 3, or any
# later version accepted by the membership of KDE e.V. (or its
# successor approved by the membership of KDE e.V.), which shall
# act as a proxy defined in Section 6 of version 3 of the license.
#
# This library is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with this library.  If not, see <http://www.gnu.org/licenses/>.

require 'rake/clean'

task 'bazaar.hpi' do |t|
  dir = 'bazaar-plugin'
  branch = 'build'
  unless File.exist?(dir)
    sh "git clone https://github.com/apachelogger/bazaar-plugin #{dir}"
  end
  Dir.chdir(dir) do
    sh 'git reset --hard'
    sh 'git clean -fd'
    sh 'git checkout master'
    sh 'git pull'
    sh "git branch -D #{branch} || true"
    sh "git checkout -b #{branch}"
    %w(origin/multiscm origin/workdir origin/version).each do |pick|
      sh "git cherry-pick #{pick}"
    end
    sh 'mvn clean install -DskipTests=true -U'
    FileUtils.cp('target/bazaar.hpi', __dir__)
  end
end
CLEAN << 'bazaar-plugin'
CLOBBER << 'bazaar.hpi'

task 'kde-oauth-plugin.hpi' do |t|
  dir = 'kde-oauth-plugin'
  unless File.exist?(dir)
    sh "git clone git://anongit.kde.org/scratch/sitter/kde-oauth-plugin.git #{dir}"
  end
  Dir.chdir(dir) do
    sh 'git reset --hard'
    sh 'git clean -fd'
    sh 'git pull'
    sh 'mvn clean install -DskipTests=true -U'
    FileUtils.cp('target/kde-oauth-plugin.hpi', __dir__)
  end
end

task :default => %w(bazaar.hpi kde-oauth-plugin.hpi)
