####################################################################################################
# Bonjour message
####################################################################################################

markdown "I'm the Danger bot, I'm here to help you doing your review"

# Sometimes it's a README fix, or something like that - which isn't relevant for
# including in a project's CHANGELOG for example

# declared_trivial = github.pr_title.include? "#trivial"

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
#warn("PR is classed as Work in Progress") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
#warn("Big PR") if git.lines_of_code > 500

# warn(vsts.pr_title)


# GITFLOW

markdown [
  "Development process",
  "==="
]

####################################################################################################
# Make it more obvious that a PR is a work in progress
####################################################################################################

if vsts.pr_title.include? "[WIP]" || gitlab.mr_title.starts_with?('WIP')
  warn("PR is classed as Work in Progress")
end

####################################################################################################
# Make it more obvious that a PR has many changes
#####################################################################################################

warn("Big PR") if git.lines_of_code > 500

####################################################################################################
# Make it more obvious that a PR has many changes
#####################################################################################################

changelog_updated = git.modified_files.include?('CHANGELOG.md')
if changelog_updated
  diff = git.diff_for_file('CHANGELOG.md')
  added_lines = diff.patch.split("\n")
    .select { |l| l.start_with?('+') }
    .reject { |l| l.start_with?('+++ b/CHANGELOG') }
  invalid_lines = added_lines
    .reject { |l| l == '+' } # empty line
    .reject { |l| l.start_with?('+## ') } # title line
    .reject { |l| l.match(entry_re)} # valid entry line
  invalid_lines.each do |l|
    warn("New CHANGELOG entries should start with \`* \` followed by a jira reference\n\`#{l}\`")
  end
else
  warn("Please include an entry in the CHANGELOG.")
end



# DOCUMENTATION

# ANDROID

####################################################################################################
# Highlight Android Lint issues on updated files
####################################################################################################

android_lint.gradle_task = "lintDevDebug"
android_lint.report_file = "./reports/lint/lint-report.xml"
android_lint.filtering = true
android_lint.lint

# KOTLIN

####################################################################################################
# Highlight Detekt issues on updated files
####################################################################################################

#kotlin_detekt.gradle_task = "detektCheckMyFlavorDebug"
#kotlin_detekt.severity = "error"
kotlin_detekt.filtering = true
kotlin_detekt.report_file  = "./reports/detekt/detekt-report.xml"
kotlin_detekt.detekt

# TEST

# BINARIES

markdown [
  "Binaries analysis",
  "==="
]

apkstats.apk_filepath='./app/build/outputs/apk/dev/debug/app-dev-debug.apk'

markdown [
    "|Charactistics|Value|",
    "|:----|:----|",
    " Taille fichier |message(apkstats.file_size)|"
]
message(apkstats.file_size)
message(apkstats.download_size)
message(apkstats.required_features)
message(apkstats.non_required_features)
message(apkstats.permissions)
message(apkstats.min_sdk)
message(apkstats.target_sdk)
# message("#{apkstats.reference_count}")
message("#{apkstats.dex_count}")

# apkanalyzer.apk_file = "./app/build/outputs/apk/dev/debug/app-dev-debug.apk"

# message (apkanalyzer.file_size)

# apkanalyzer.file_size
# Print permissions used by application

# message (apkanalyzer.permissions)
# Print number of method references

# message (apkanalyzer.method_references)