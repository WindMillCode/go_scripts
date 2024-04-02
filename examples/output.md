# FileName: \array_utils.go 

// 19 types
package utils


func RemoveElementsNotInSource[T comparable](source, toRemove []T) []T {
	sourceSet := make(map[T]bool)
	for _, value := range source {
			sourceSet[value] = true
	}

	var result []T
	for _, value := range toRemove {
			if !sourceSet[value] {
					result = append(result, value)
			}
	}

	return result
}


func ArrayContainsAny(contentSlice, targetSlice []string) bool {
	for _, content := range contentSlice {
		for _, target := range targetSlice {
			if content == target {
				return true // Return true immediately if a match is found
			}
		}
	}
	return false // Return false if no matches are found
}

func FilterArray[T any](arr []T, condition func(interface{},int) bool) []interface{} {
	var filtered []interface{}
	for index, element := range arr {
			if condition(element,index) {
					filtered = append(filtered, element)
			}
	}
	return filtered
}

func ConvertToInterfaceArray[T any](input []T) []interface{} {
	var result []interface{}
	for _, v := range input {
			result = append(result, v)
	}
	return result
}

func ConvertToIntArray(input []interface{}) []int {
    var result []int
    for _, v := range input {
        if num, ok := v.(int); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToUintArray(input []interface{}) []uint {
    var result []uint
    for _, v := range input {
        if num, ok := v.(uint); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToInt8Array(input []interface{}) []int8 {
    var result []int8
    for _, v := range input {
        if num, ok := v.(int8); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToUint8Array(input []interface{}) []uint8 {
    var result []uint8
    for _, v := range input {
        if num, ok := v.(uint8); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToInt16Array(input []interface{}) []int16 {
    var result []int16
    for _, v := range input {
        if num, ok := v.(int16); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToUint16Array(input []interface{}) []uint16 {
    var result []uint16
    for _, v := range input {
        if num, ok := v.(uint16); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToInt32Array(input []interface{}) []int32 {
    var result []int32
    for _, v := range input {
        if num, ok := v.(int32); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToUint32Array(input []interface{}) []uint32 {
    var result []uint32
    for _, v := range input {
        if num, ok := v.(uint32); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToInt64Array(input []interface{}) []int64 {
    var result []int64
    for _, v := range input {
        if num, ok := v.(int64); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToUint64Array(input []interface{}) []uint64 {
    var result []uint64
    for _, v := range input {
        if num, ok := v.(uint64); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToFloat32Array(input []interface{}) []float32 {
    var result []float32
    for _, v := range input {
        if num, ok := v.(float32); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToFloat64Array(input []interface{}) []float64 {
    var result []float64
    for _, v := range input {
        if num, ok := v.(float64); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToComplex64Array(input []interface{}) []complex64 {
    var result []complex64
    for _, v := range input {
        if num, ok := v.(complex64); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToComplex128Array(input []interface{}) []complex128 {
    var result []complex128
    for _, v := range input {
        if num, ok := v.(complex128); ok {
            result = append(result, num)
        }
    }
    return result
}

func ConvertToBoolArray(input []interface{}) []bool {
    var result []bool
    for _, v := range input {
        if b, ok := v.(bool); ok {
            result = append(result, b)
        }
    }
    return result
}

func ConvertToStringArray(input []interface{}) []string {
	var result []string
	for _, v := range input {
			if str, ok := v.(string); ok {
					result = append(result, str)
			}
	}
	return result
}

func ConvertToByteArray(input []interface{}) []byte {
	var result []byte
	for _, v := range input {
			if b, ok := v.(byte); ok {
					result = append(result, b)
			}
	}
	return result
}

func ConvertToRuneArray(input []interface{}) []rune {
	var result []rune
	for _, v := range input {
			if r, ok := v.(rune); ok {
					result = append(result, r)
			}
	}
	return result
}
# FileName: \async_utils.go 

package utils

import "fmt"

func DecreaseChannelBatchFn(i int, batchSize int, batchDone chan bool, targetArray []string) {
	if i%batchSize == 0 {

		for j := 0; j < batchSize; j++ {
			<-batchDone
		}
		fmt.Println("Batch complete")
	} else if i == len(targetArray) {

		for j := 0; j < len(targetArray)%batchSize; j++ {
			<-batchDone
		}
		fmt.Println("Batch complete")
	}
}
# FileName: \cd_utils.go 

package utils

import (
	"os"
	"path/filepath"
)

func CDToLocation(location string, opts ...interface{}) {
	createIfNotExist := false

	if len(opts) > 0 {
		// Expecting the first option to be a boolean
		if create, ok := opts[0].(bool); ok {
			createIfNotExist = create
		}
	}

	if _, err := os.Stat(location); os.IsNotExist(err) {
		if createIfNotExist {
			// Create the directory if it does not exist
			if mkErr := os.MkdirAll(location, os.ModePerm); mkErr != nil {
				panic(mkErr) // Panic if unable to create the directory
			}
		} else {
			panic("Destination does not exist")
		}
	}

	// Change directory
	if err := os.Chdir(location); err != nil {
		panic(err) // Panic if unable to change directory
	}
}
func CDToWorkspaceRoot() {
	CDToLocation(filepath.Join("..", "..", ".."))
}
func CDToAngularApp() {
	CDToLocation(filepath.Join("apps", "frontend", "AngularApp"))
}

func CDToFirebaseApp() {
	CDToLocation(filepath.Join("apps", "cloud", "FirebaseApp"))
}

func CDToFlaskApp() {
	CDToLocation(filepath.Join("apps", "backend", "FlaskApp"))
}

func CDToTestNGApp() {
	CDToLocation(filepath.Join("apps", "testing", "testng"))
}

func CDToFlutterApp() {
	CDToLocation(filepath.Join("apps", "mobile", "FlutterApp"))
}



# FileName: \common_utils.go 

package utils

import (
	"fmt"
	"os"
	"os/exec"
	"path/filepath"
	"reflect"
	"strings"
)



// getType returns the type of a given value as a string
func GetType(value interface{}) string {
	return reflect.TypeOf(value).String()
}


func GetParamValue(parameterName string, parameterValue interface{}) interface{} {
	if parameterValue != nil {
		return parameterValue
	} else {
		fmt.Printf("Parameter '%s' value not found.\n", parameterName)
		return nil
	}
}

func GetCurrentPath() string {
	executablePath, err := os.Executable()
	if err != nil {
		// Handle the error if necessary
		return ""
	}
	return filepath.Dir(executablePath)
}

func GetCurrentBranch() (string, error) {
	cmd := exec.Command("git", "rev-parse", "--abbrev-ref", "HEAD")
	output, err := cmd.Output()
	if err != nil {
		return "", err
	}

	// Remove leading/trailing whitespaces and newline characters
	branch := strings.TrimSpace(string(output))

	return branch, nil
}


// Function to clear the console screen
func ClearScreen() {
	fmt.Print("\033[H\033[2J")
}
# FileName: \copy_utils.go 

package utils

import (
	"fmt"
	"io"
	"os"
	"path/filepath"
)

func CopyDir(src, dest string) error {
	srcInfo, err := os.Stat(src)
	if err != nil {
		return err
	}

	// Create the destination directory with the same permissions as the source
	if err := os.MkdirAll(dest, srcInfo.Mode()); err != nil {
		return err
	}

	entries, err := os.ReadDir(src)
	if err != nil {
		return err
	}

	// If the directory is empty, we are done as MkdirAll has already created the destination directory
	if len(entries) == 0 {
		return nil
	}

	for _, entry := range entries {
		srcPath := filepath.Join(src, entry.Name())
		destPath := filepath.Join(dest, entry.Name())

		fileInfo, err := entry.Info()
		if err != nil {
			return err
		}

		if fileInfo.IsDir() {
			// Recursively copy the subdirectory
			if err := CopyDir(srcPath, destPath); err != nil {
				return err
			}
		} else {
			// Copy the file
			if err := CopyFile(srcPath, destPath); err != nil {
				return err
			}
		}
	}

	return nil
}

func CopyFile(src, dest string) error {
	srcFile, err := os.Open(src)
	if err != nil {
		return err
	}
	defer srcFile.Close()

	destFile, err := os.Create(dest)
	if err != nil {
		return err
	}
	defer destFile.Close()

	_, err = io.Copy(destFile, srcFile)
	if err != nil {
		return err
	}

	return nil
}



type CopySelectFilesToDestinationStruct struct {
	SourceFiles  []string
	GlobPattern string //regex
	DestinationDir string
}
func CopySelectFilesToDestination(c CopySelectFilesToDestinationStruct) error {



	// Move files with the specified pattern to the destination directory
	if c.GlobPattern !=""{
		var err error
		c.SourceFiles,err = filepath.Glob(c.GlobPattern)
		if err != nil {
				return err
		}
	}


	for _, file := range c.SourceFiles {
			if err := CopyFile(file, filepath.Join(c.DestinationDir, filepath.Base(file))); err != nil {
					return err
			}
	}

	return nil
}


func main() {
	srcDir := "/path/to/source/directory"
	destDir := "/path/to/destination/directory"

	err := CopyDir(srcDir, destDir)
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	fmt.Println("Directory copied successfully.")
}
# FileName: \docker-utils.go 

package utils

import (
	"bufio"
	"os"
	"strings"
)




func IsRunningInDocker() bool {

	if _, exists := os.LookupEnv("REMOTE_CONTAINERS_IPC"); exists {
		return true
	}

	if _, exists := os.LookupEnv("REMOTE_CONTAINERS_SOCKETS"); exists {
		return true
	}

	if _, exists := os.LookupEnv("REMOTE_CONTAINERS_DISPLAY_SOCK"); exists {
		return true
	}

	if _, exists := os.LookupEnv("REMOTE_CONTAINERS"); exists {
		return true
	}



	if _, err := os.Stat("/.dockerenv"); err == nil {
		return true
	}



	file, err := os.Open("/proc/1/cgroup")
	if err != nil {
		return false
	}
	defer file.Close()

	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		if strings.Contains(scanner.Text(), "docker") {
			return true
		}
	}

	return false
}
# FileName: \file_utils.go 

package utils

import (
	"bufio"
	"fmt"
	"io"
	"io/fs"
	"log"
	"net/http"
	"os"
	"path/filepath"
	"regexp"
	"runtime"
	"strings"
	"time"

	"github.com/fsnotify/fsnotify"
	"github.com/gobwas/glob"
)


func FilterFilesByExtension(files []string, extensions []string, include bool) []string {
	var filteredFiles []string

	// Create a map for faster extension checks
	extensionMap := make(map[string]bool)
	for _, ext := range extensions {
			extensionMap[ext] = true
	}

	// Iterate over the files and filter based on the extension and include flag
	for _, file := range files {
			_, extPresent := extensionMap[strings.ToLower(strings.TrimPrefix(filepath.Ext(file), "."))]

			// If include is true, keep the file if the extension is in the list
			// If include is false, keep the file if the extension is not in the list
			if (include && extPresent) || (!include && !extPresent) {
					filteredFiles = append(filteredFiles, file)
			}
	}

	return filteredFiles
}

func ReadFile(filePath string) (string, error) {
	// Read the entire content of the file
	content, err := os.ReadFile(filePath)
	if err != nil {
		fmt.Printf("Error reading from file %s", err)
		return "", err
	}

	// Convert content to a string and return it
	return string(content), nil
}

func OverwriteFile(filePath string, content string) error {
	err := os.WriteFile(filePath, []byte(content), 0644)
	if err != nil {
		fmt.Printf("Error writing to file", err)
	}
	return err
}

func FolderExists(path string) bool {
	_, err := os.Stat(path)
	if !os.IsNotExist(err) {
		// path/to/whatever exists
		return true
	} else {
		return false
	}
}

func GetItemsInFolder(folderPath string) ([]string, error) {
	files, err := os.ReadDir(folderPath)
	if err != nil {
		return nil, err
	}

	var filenames []string
	for _, file := range files {
		// if !file.IsDir() {
		filenames = append(filenames, file.Name())
		// }
	}

	return filenames, nil
}

func GetItemsInFolderRecursive(folderPath string, recursive bool) ([]string, error) {
	var filenames []string

	err := filepath.WalkDir(folderPath, func(path string, d fs.DirEntry, err error) error {
		if err != nil {
			return err
		}

		if !recursive && path != folderPath {
			return filepath.SkipDir // Skip subdirectories when not in recursive mode
		}

		filenames = append(filenames, path)

		return nil
	})

	if err != nil {
		return nil, err
	}

	return filenames, nil
}

func HasSuffixInArray(str string, suffixes []string, removeSuffix bool) string {
	for _, suffix := range suffixes {
		if strings.HasSuffix(str, suffix) {
			if removeSuffix == true {

				return strings.TrimSuffix(str, suffix)
			} else {
				return str
			}
		}
	}
	return ""
}

func HasPrefixInArray(str string, prefixes []string, removeSuffix bool) string {
	for _, prefix := range prefixes {
		if strings.HasPrefix(str, prefix) {
			if removeSuffix == true {

				return strings.TrimPrefix(str, prefix)
			} else {
				return str
			}
		}
	}
	return ""
}

func RemoveDrivePath(folderPath string) string {

	folderPath = filepath.ToSlash(folderPath)
	parts := strings.Split(folderPath, "/")
	if len(parts) >= 2 && strings.HasSuffix(parts[0], ":") {

		parts = parts[1:]
	}
	resultPath := filepath.Join(parts...)
	resultPath = filepath.FromSlash(resultPath)
	return resultPath
}

func IsFileOrFolder(path string) (string, error) {
	fileInfo, err := os.Stat(path)
	if err != nil {
		return "", err
	}

	if fileInfo.IsDir() {
		return "dir", nil
	}

	return "file", nil
}

func ConvertPathToOSFormat(inputPath string) string {
	return filepath.FromSlash(inputPath)
}

func JoinAndConvertPathToOSFormat(inputPathParts ...string) string {
	inputPath := filepath.Join(inputPathParts...)
	return ConvertPathToOSFormat(inputPath)
}

func ProcessFilesMatchingPattern(directory, pattern string, predicateFn func(string)) error {
	// Compile the regular expression pattern
	regex, err := regexp.Compile(pattern)
	if err != nil {
		return err
	}

	// Walk the directory and apply the predicate function to matching files
	err = filepath.Walk(directory, func(path string, info os.FileInfo, err error) error {
		if err != nil {
			return err
		}

		if !info.IsDir() && regex.MatchString(info.Name()) {
			// Apply the predicate function to the full path of the matching file
			// fmt.Println(path)
			predicateFn(path)
		}

		return nil
	})

	return err
}

func ProcessFoldersMatchingPattern(directory, pattern string, predicateFn func(string)) error {
	// Compile the regular expression pattern
	regex, err := regexp.Compile(pattern)
	if err != nil {
		return err
	}

	// Walk the directory and apply the predicate function to matching files
	err = filepath.Walk(directory, func(path string, info os.FileInfo, err error) error {
		if err != nil {
			return err
		}

		if info.IsDir() && regex.MatchString(info.Name()) {
			// Apply the predicate function to the full path of the matching file
			// fmt.Println(path)
			predicateFn(path)
		}

		return nil
	})

	return err
}

func AddContentToFile(filePath, valueToAdd string,positon string) error {
	// Read the original file content
	originalContent, err := os.ReadFile(filePath)
	if err != nil {
		return err
	}

	// Prepend the prefix to the content
	var newContent []byte
	if positon == "prefix"{
		newContent = []byte(valueToAdd + string(originalContent))
	} else {
		newContent = []byte(  string(originalContent)+valueToAdd)
	}

	// Write the modified content back to the file
	err = os.WriteFile(filePath, newContent, os.ModePerm)
	if err != nil {
		return err
	}

	fmt.Printf("Modified file: %s\n", filePath)
	return nil
}

func AddContentToEachLineInFile(filePath string, predicate func(string) string) error {
	// Open the file for reading and writing
	file, err := os.OpenFile(filePath, os.O_RDWR, os.ModePerm)
	if err != nil {
		return err
	}
	defer file.Close()

	// Create a temporary file to store modified content
	tempFile, err := os.CreateTemp("", "tempfile")
	if err != nil {
		return err
	}
	defer tempFile.Close()

	// Create a scanner to read from the original file
	scanner := bufio.NewScanner(file)

	// Create a writer for the temporary file
	writer := bufio.NewWriter(tempFile)

	for scanner.Scan() {
		line := scanner.Text()
		newLine := predicate(line)

		// Write the modified line to the temporary file
		_, err := writer.WriteString(newLine + "\n")
		if err != nil {
			return err
		}
	}

	if err := scanner.Err(); err != nil {
		return err
	}

	// Flush any remaining data to the temporary file
	if err := writer.Flush(); err != nil {
		return err
	}

	// Close both files before replacing the original with the temporary file
	file.Close()
	tempFile.Close()

	// Replace the original file with the temporary file
	err = os.Rename(tempFile.Name(), filePath)
	if err != nil {
		return err
	}

	return nil
}

func RemoveContentFromFile(filePath string, contentToRemove []string) error {
	file, err := os.Open(filePath)
	if err != nil {
		return err
	}
	defer file.Close()

	var lines []string
	scanner := bufio.NewScanner(file)
LineLoop:
	for scanner.Scan() {
		line := scanner.Text()
		for _, remove := range contentToRemove {
			if strings.TrimSpace(line) == remove {
				continue LineLoop // Skip this line as it matches one of the removal strings
			}
		}
		lines = append(lines, line) // Keep the line if no matches were found
	}

	if err := scanner.Err(); err != nil {
		return err
	}

	return os.WriteFile(filePath, []byte(strings.Join(lines, "\n")), 0644)
}

func MergeDirectories(sourceDir, targetDir string, overwrite bool) error {
	return filepath.Walk(sourceDir, func(srcPath string, info os.FileInfo, err error) error {
		if err != nil {
			return err
		}

		relPath, err := filepath.Rel(sourceDir, srcPath)
		if err != nil {
			return err
		}

		destPath := filepath.Join(targetDir, relPath)

		if info.IsDir() {
			if err := os.MkdirAll(destPath, os.ModePerm); err != nil {
				return err
			}
		} else {
			_, err := os.Stat(destPath)
			if err == nil && !overwrite {
				return nil
			}

			if err := CopyFile(srcPath, destPath); err != nil {
				return err
			}
		}

		return nil
	})
}

type TraverseDirectoryParams struct {
	RootDir   string
	Predicate func(string, os.FileInfo)
	Filter    func(string, os.FileInfo) bool
}

func TraverseDirectory(config TraverseDirectoryParams) error {
	return filepath.Walk(config.RootDir, func(path string, info os.FileInfo, err error) error {
		if err != nil {
			return err // Return any error that occurs during traversal
		}

		// Apply the filter function if provided
		if config.Filter != nil && !config.Filter(path, info) {
			return nil // Skip entries that don't match the filter
		}

		config.Predicate(path, info)
		return nil
	})
}

func DownloadFile(url, localPath string) error {
	outFile, err := os.Create(localPath)
	if err != nil {
			return fmt.Errorf("error creating file: %v", err)
	}
	defer outFile.Close()

	response, err := http.Get(url)
	if err != nil {
			return fmt.Errorf("error making GET request: %v", err)
	}
	defer response.Body.Close()

	if response.StatusCode != http.StatusOK {
			return fmt.Errorf("bad status: %s", response.Status)
	}

	_, err = io.Copy(outFile, response.Body)
	if err != nil {
			return fmt.Errorf("error writing to file: %v", err)
	}

	return nil
}

func ExtractArchive(archiveURL string,removeArchiveFile bool) string {
	// Get the filename from the URL
	segments := strings.Split(archiveURL, "/")
	filename := segments[len(segments)-1]

	// Get the current working directory
	sourceDir, err := GetSourceFilePath()
	if err != nil {
			return fmt.Sprintf("error getting source file directory: %v", err)
	}

	// Construct the full path for the archive
	archivePath := JoinAndConvertPathToOSFormat(sourceDir, filename)

	// Check if the file exists locally, if not, download it
	if _, err := os.Stat(archivePath); os.IsNotExist(err) {
			fmt.Printf("File not found locally. Downloading from %s\n", archiveURL)
			if err := DownloadFile(archiveURL, archivePath); err != nil {
					return fmt.Sprintf("error downloading file: %v", err)
			}
	}

	// Extract the archive using 7z
	sevenZCommandOptions :=CommandOptions{
		Command: "7z",
		Args: []string{"x",archivePath,"-aoa"},
		TargetDir: filepath.Dir(archivePath),
	}
	RunCommandWithOptions(sevenZCommandOptions)

	fmt.Printf("Archive extracted successfully: %s\n", archivePath)
	if removeArchiveFile == true{
		if err := os.Remove(archivePath); err != nil {
			return fmt.Sprintf("error removing archive file: %v", err)
	}

	fmt.Println("Archive file deleted successfully")
	}

	return filepath.Dir(archivePath)
}

func GetSourceFilePath() (string, error) {
	executable, err := os.Executable()
	if err != nil {
			return "", fmt.Errorf("unable to get this programs executable path")
	}
	return filepath.Dir(executable), nil
}

func FindExecutable(executablePrefix, searchDir string) string {
	var executablePath string
	var found bool

	// Define the executable name pattern based on the OS
	executablePattern := executablePrefix
	if runtime.GOOS == "windows" {
			executablePattern += ".exe"
	}

	// Define the filter function to limit the search to executable files
	filterFunc := func(path string, info os.FileInfo) bool {
			return !info.IsDir() && filepath.Base(path) == executablePattern
	}

	// Define the predicate function to capture the path of the first matching file
	predicateFunc := func(path string, info os.FileInfo) {
			executablePath = path
			found = true
	}

	// Traverse the directory
	err := TraverseDirectory(
		TraverseDirectoryParams{
			RootDir:   searchDir,
			Predicate: predicateFunc,
			Filter:    filterFunc,
		},
	)

	if err != nil {
			return  fmt.Sprintf("error traversing directory: %v", err)
	}

	if !found {
			return  fmt.Sprintf("NOTFOUND")
	}

	return executablePath
}


type WatchDirectoryParams struct {
	DirectoryToWatch string
	DebounceInMs     int
	Predicate        func(event fsnotify.Event)
	StartOnWatch     bool
	IncludePatterns  []string
	ExcludePatterns  []string
}

func WatchDirectory(options WatchDirectoryParams) {
	watcher, err := fsnotify.NewWatcher()
	if err != nil {
		log.Fatal(err)
	}
	defer watcher.Close()

	// Compile glob patterns
	includeGlobs := CompileGlobs(options.IncludePatterns)
	excludeGlobs := CompileGlobs(options.ExcludePatterns)

	// Function to check if a path should be included or excluded
	shouldIncludePath := func(path string) bool {
		if len(excludeGlobs) > 0 && MatchAnyGlob(excludeGlobs, path) {
			return false
		}
		if len(includeGlobs) == 0 || MatchAnyGlob(includeGlobs, path) {
			return true
		}
		return false
	}

	// Call the Predicate immediately for each file if StartOnWatch is true
	if options.StartOnWatch {
		// Manually creating a test event
		testEvent := fsnotify.Event{Name: options.DirectoryToWatch, Op: fsnotify.Write}
		options.Predicate(testEvent)
	}


	// Setup the watcher
	if err := filepath.Walk(options.DirectoryToWatch,
		func(path string, fi os.FileInfo, err error) error {
			if shouldIncludePath(path) {
				if fi.Mode().IsDir() {
					return watcher.Add(path)
				}
			}

			return nil
		},
	); err != nil {
		fmt.Println("ERROR", err)
	}

	fmt.Printf("Watching directory %s\n", options.DirectoryToWatch)

	done := make(chan bool)

	go func() {
		var lastEventTime time.Time

		for {
			select {
			case event, ok := <-watcher.Events:
				if !ok {
					return
				}

				// Calculate the time elapsed since the last event
				elapsedTime := time.Since(lastEventTime)
				if int(elapsedTime.Milliseconds()) >= options.DebounceInMs {
					options.Predicate(event)
				}

				// Update the last event time
				lastEventTime = time.Now()

			case err, ok := <-watcher.Errors:
				if !ok {
					return
				}
				fmt.Println("ERROR", err)
			}
		}
	}()

	<-done
}

// CompileGlobs compiles the string patterns into glob.Glob objects.
func CompileGlobs(patterns []string) []glob.Glob {
	var globs []glob.Glob
	for _, pattern := range patterns {
		g, err := glob.Compile(pattern)
		if err == nil {
			globs = append(globs, g)
		}
	}
	return globs
}

// MatchAnyGlob checks if a path matches any of the provided glob patterns.
func MatchAnyGlob(globs []glob.Glob, path string) bool {
	for _, g := range globs {
		if g.Match(ConvertPathToOSFormat(path)) {
			return true
		}
	}
	return false
}

func RemovePathPrefix(path string, prefixArray []string) string {
	for _, prefix := range prefixArray {
			if strings.HasPrefix(path, prefix) {
					return strings.TrimPrefix(path, prefix)
			}
	}
	return path // Return the original path if no prefix matches
}

func EnsureDirAndCreateFile(filePath string) (*os.File, error) {
	dir := filepath.Dir(filePath)

	// Create all directories in the path if they don't exist
	if err := os.MkdirAll(dir, 0755); err != nil {
		return nil, fmt.Errorf("failed to create directory: %w", err)
	}

	// Create the file
	file, err := os.Create(filePath)
	if err != nil {
		return nil, fmt.Errorf("failed to create file: %w", err)
	}

	return file, nil
}
# FileName: \git-utils.go 

package utils

import (
	"fmt"
	"os"
	"os/exec"
	"path/filepath"
	"strings"
)


func GitSparseClone(repoURL string, localDir string, subdirectories ...string) error {
	// Create the local directory if it doesn't exist
	if err := os.MkdirAll(localDir, os.ModePerm); err != nil {
		return err
	}

	// Change to the local directory
	if err := os.Chdir(localDir); err != nil {
		return err
	}

	// Initialize a Git repository
	cmd := exec.Command("git", "init")
	if err := cmd.Run(); err != nil {
		return err
	}

	// Add a remote and fetch
	cmd = exec.Command("git", "remote", "add", "-f", "origin", repoURL)
	if err := cmd.Run(); err != nil {
		return err
	}

	// Configure sparse checkout
	cmd = exec.Command("git", "config", "core.sparseCheckout", "true")
	if err := cmd.Run(); err != nil {
		return err
	}

	// Write subdirectories to .git/info/sparse-checkout
	sparseFile := filepath.Join(".git", "info", "sparse-checkout")
	file, err := os.Create(sparseFile)
	if err != nil {
		return err
	}
	defer file.Close()

	for _, subdir := range subdirectories {
		fmt.Fprintln(file, subdir)
	}

	// Pull from the remote repository
	cmd = exec.Command("git", "pull", "origin", "master")
	if err := cmd.Run(); err != nil {
		return err
	}

	return nil
}

func ExtractBranchNames(input string) []string {
	fmt.Printf(input)
	var branchNames []string

    lines := strings.Split(input, "[new branch]")

    for _, line := range lines {
        parts := strings.Fields(line)
        if len(parts) >= 4 && parts[3] == "->" {
            branchName := strings.TrimSpace(parts[2])
            branchNames = append(branchNames, branchName)
        }
    }

    return branchNames
}
# FileName: \github_utils.go 

package utils

import (
	"encoding/json"
	"fmt"
	"io"
	"net/http"
	"runtime"
	"strings"
)

type GitHubRelease struct {
	TagName string `json:"tag_name"`
	Assets  []struct {
		Name               string `json:"name"`
		BrowserDownloadURL string `json:"browser_download_url"`
	} `json:"assets"`
}

func GetLatestRelease(repoURL string) (*GitHubRelease, error) {
	// GitHub API URL for the latest release
	apiURL := repoURL + "/releases/latest"

	// Make the HTTP request
	resp, err := http.Get(apiURL)
	if err != nil {
		return nil, fmt.Errorf("error making request: %v", err)
	}
	defer resp.Body.Close()

	// Read the response body
	body, err := io.ReadAll(resp.Body)
	if err != nil {
		return nil, fmt.Errorf("error reading response body: %v", err)
	}

	// Unmarshal the JSON data into the GitHubRelease struct
	var release GitHubRelease
	if err := json.Unmarshal(body, &release); err != nil {
		return nil, fmt.Errorf("error unmarshaling JSON: %v", err)
	}

	// Return the entire release struct
	return &release, nil
}

func GetDownloadURLForCurrentOS(release *GitHubRelease) (string, error) {
	if release == nil || len(release.Assets) == 0 {
		return "", fmt.Errorf("no assets found in the release")
	}

	// Determine the current operating system and architecture
	osName := runtime.GOOS
	arch := runtime.GOARCH

	// Define alternate names for architectures
	archMappings := map[string][]string{
		"amd64": {"amd64", "x86_64", "x64"},
		"386":   {"386", "x86"},
		"arm64": {"arm64", "aarch64"},
		"arm":   {"arm"},
	}

	// Get the possible alternate names for the architecture
	archAlternates, ok := archMappings[arch]
	if !ok {
		return "", fmt.Errorf("no alternate names found for architecture: %s", arch)
	}

	// Loop through the assets to find a match for the current OS and one of the architecture alternates
	for _, asset := range release.Assets {
		assetNameLower := strings.ToLower(asset.Name)
		if strings.Contains(assetNameLower, osName) && ContainsAny(assetNameLower, archAlternates) {
			return asset.BrowserDownloadURL, nil
		}
	}

	return "", fmt.Errorf("no zip asset found for the OS: %s and architecture: %s", osName, arch)
}
# FileName: \json_utils.go 

package utils

import (
	"bytes"
	"encoding/json"
	"fmt"
	"os"

)



func FilterJSONByPredicate(inputJSON []byte, predicate func(key string, value interface{}) bool) ([]byte, error) {
	var jsonData map[string]interface{}

	if err := json.Unmarshal(inputJSON, &jsonData); err != nil {
		return nil, err
	}

	filteredData := make(map[string]interface{})

	for key, value := range jsonData {
		if predicate(key, value) {
			filteredData[key] = value
		}
	}

	filteredJSON, err := json.Marshal(filteredData)
	if err != nil {
		return nil, err
	}

	return filteredJSON, nil
}



func WriteCustomFormattedJSONToFile(data interface{}, filename string, indentString string) error {
	file, err := os.Create(filename)
	if err != nil {
		return err
	}
	defer file.Close()

	var dataBytes []byte
	var out bytes.Buffer
	switch d := data.(type) {
	case []byte:
		dataBytes = d

	default:
		var err error
		dataBytes, err = json.MarshalIndent(d, "", indentString)
		if err != nil {
			return err
		}
	}
	json.Indent(&out, UnicodeUnquote(dataBytes), "", indentString)

	out.WriteTo(file)
	if err != nil {
		return err
	}

	return nil
}
func WriteFormattoJSONFile(data interface{}, filename string){
	WriteCustomFormattedJSONToFile(data,filename,"    ")
}



func RemoveComments(data []byte) ([]byte, error) {
	inString := false
	inSingleLineComment := false
	inMultiLineComment := false
	var result []byte

	for i := 0; i < len(data); i++ {
		if inSingleLineComment {
			if data[i] == '\n' {
				inSingleLineComment = false
			}
			continue
		}

		if inMultiLineComment {
			if data[i] == '*' && i+1 < len(data) && data[i+1] == '/' {
				inMultiLineComment = false
				i++ // Skip the '/'
			}
			continue
		}

		if inString {
			if data[i] == '"' && data[i-1] != '\\' {
				inString = false
			}
			result = append(result, data[i])
			continue
		}

		if data[i] == '"' {
			inString = true
			result = append(result, data[i])
			continue
		}

		if data[i] == '/' && i+1 < len(data) {
			if data[i+1] == '/' {
				inSingleLineComment = true
				i++ // Skip the next '/'
				continue
			} else if data[i+1] == '*' {
				inMultiLineComment = true
				i++ // Skip the next '*'
				continue
			}
		}

		result = append(result, data[i])
	}

	if !json.Valid(result) {
		return nil, fmt.Errorf("invalid JSON after removing comments")
	}

	return result, nil
}
# FileName: \map_utils.go 

package utils

func OverwriteMap[T any](targetMap, newMap map[string]T) {
	for key, value := range newMap {
			targetMap[key] = value
	}
}

func FilterMap[T any](originalMap map[string]T, predicate func(string,T) bool) map[string]interface{} {
	filteredMap := make(map[string]interface{})

	for key, value := range originalMap {
			if predicate(key,value) {
					filteredMap[key] = value
			}
	}

	return filteredMap
}

func ConvertToStringIntMap(originalMap map[string]interface{}) map[string]int {
	intMap := make(map[string]int)

	for key, value := range originalMap {
			if intValue, ok := value.(int); ok {
					intMap[key] = intValue
			}
	}

	return intMap
}

func ConvertToStringInt8Map(originalMap map[string]interface{}) map[string]int8 {
	int8Map := make(map[string]int8)

	for key, value := range originalMap {
			if intValue, ok := value.(int8); ok {
					int8Map[key] = intValue
			}
	}

	return int8Map
}

func ConvertToStringInt16Map(originalMap map[string]interface{}) map[string]int16 {
	int16Map := make(map[string]int16)

	for key, value := range originalMap {
			if intValue, ok := value.(int16); ok {
					int16Map[key] = intValue
			}
	}

	return int16Map
}

func ConvertToStringInt32Map(originalMap map[string]interface{}) map[string]int32 {
	int32Map := make(map[string]int32)

	for key, value := range originalMap {
			if intValue, ok := value.(int32); ok {
					int32Map[key] = intValue
			}
	}

	return int32Map
}

func ConvertToStringInt64Map(originalMap map[string]interface{}) map[string]int64 {
	int64Map := make(map[string]int64)

	for key, value := range originalMap {
			if intValue, ok := value.(int64); ok {
					int64Map[key] = intValue
			}
	}

	return int64Map
}

func ConvertToStringUint8Map(originalMap map[string]interface{}) map[string]uint8 {
	uint8Map := make(map[string]uint8)

	for key, value := range originalMap {
			if uintValue, ok := value.(uint8); ok {
					uint8Map[key] = uintValue
			}
	}

	return uint8Map
}

func ConvertToStringUint16Map(originalMap map[string]interface{}) map[string]uint16 {
	uint16Map := make(map[string]uint16)

	for key, value := range originalMap {
			if uintValue, ok := value.(uint16); ok {
					uint16Map[key] = uintValue
			}
	}

	return uint16Map
}

func ConvertToStringUint32Map(originalMap map[string]interface{}) map[string]uint32 {
	uint32Map := make(map[string]uint32)

	for key, value := range originalMap {
			if uintValue, ok := value.(uint32); ok {
					uint32Map[key] = uintValue
			}
	}

	return uint32Map
}

func ConvertToStringUint64Map(originalMap map[string]interface{}) map[string]uint64 {
	uint64Map := make(map[string]uint64)

	for key, value := range originalMap {
			if uintValue, ok := value.(uint64); ok {
					uint64Map[key] = uintValue
			}
	}

	return uint64Map
}

func ConvertToStringFloat32Map(originalMap map[string]interface{}) map[string]float32 {
	float32Map := make(map[string]float32)

	for key, value := range originalMap {
			if floatValue, ok := value.(float32); ok {
					float32Map[key] = floatValue
			}
	}

	return float32Map
}

func ConvertToStringFloat64Map(originalMap map[string]interface{}) map[string]float64 {
	float64Map := make(map[string]float64)

	for key, value := range originalMap {
			if floatValue, ok := value.(float64); ok {
					float64Map[key] = floatValue
			}
	}

	return float64Map
}

func ConvertToStringComplex64Map(originalMap map[string]interface{}) map[string]complex64 {
	complex64Map := make(map[string]complex64)

	for key, value := range originalMap {
			if complexValue, ok := value.(complex64); ok {
					complex64Map[key] = complexValue
			}
	}

	return complex64Map
}

func ConvertToStringComplex128Map(originalMap map[string]interface{}) map[string]complex128 {
	complex128Map := make(map[string]complex128)

	for key, value := range originalMap {
			if complexValue, ok := value.(complex128); ok {
					complex128Map[key] = complexValue
			}
	}

	return complex128Map
}

func ConvertToStringBoolMap(originalMap map[string]interface{}) map[string]bool {
	boolMap := make(map[string]bool)

	for key, value := range originalMap {
			if boolValue, ok := value.(bool); ok {
					boolMap[key] = boolValue
			}
	}

	return boolMap
}

func ConvertToStringInterfaceMap(originalMap map[string]interface{}) map[string]string {
	stringMap := make(map[string]string)

	for key, value := range originalMap {
			if strValue, ok := value.(string); ok {
					stringMap[key] = strValue
			}
	}

	return stringMap
}

func ConvertToStringStringMap(originalMap map[string]interface{}) map[string]string {
	stringMap := make(map[string]string)

	for key, value := range originalMap {
			if strValue, ok := value.(string); ok {
					stringMap[key] = strValue
			}
	}

	return stringMap
}

func ConvertToStringErrorMap(originalMap map[string]interface{}) map[string]error {
	errorMap := make(map[string]error)

	for key, value := range originalMap {
			if errValue, ok := value.(error); ok {
					errorMap[key] = errValue
			}
	}

	return errorMap
}

# FileName: \show_menu.go 

package utils

import (
	"fmt"
	"os"

	"github.com/charmbracelet/bubbles/textinput"
	tea "github.com/charmbracelet/bubbletea"
)

type ShowMenuModel struct {
	Default     string
	Other       bool
	OtherString string
	Prompt      string
	Choices     []string       // items on the to-do list
	Selected    map[int]string // which to-do items are selected
	cursor      int            // which to-do list item our cursor is pointing at
	typing     bool           // if the user is typing
	textInput   textinput.Model
}

func ShowMenu(cliInfo ShowMenuModel, enableOtherOption interface{}) string {
	cliInfo.Selected = make(map[int]string)

	if cliInfo.OtherString == "" {
		cliInfo.OtherString = "Other: "
	}
	if cliInfo.Other  {
		cliInfo.Choices = append(cliInfo.Choices, cliInfo.OtherString)
	}
	if cliInfo.Default != "" {
		for i, v := range cliInfo.Choices {
			if v == cliInfo.Default {
				cliInfo.Selected[i] = cliInfo.Default
			}
		}
	}
	cliInfo.textInput = textinput.New()
	p := tea.NewProgram(cliInfo)
	if _, err := p.Run(); err != nil {
		fmt.Printf("Alas, there's been an error: %v", err)
		os.Exit(1)
	}
	var value string
	for _, v := range cliInfo.Selected {
		value = v
		break
	}
	return value
}

func (m ShowMenuModel) Init() tea.Cmd {
	return textinput.Blink
}

func (m ShowMenuModel) Update(msg tea.Msg) (tea.Model, tea.Cmd) {

	var cmd tea.Cmd = nil
	if m.typing {
		var newCmd tea.Cmd
		if  stringMsg,ok := msg.(tea.KeyMsg); ok{
			if stringMsg.String() != "enter"{
				m.textInput,newCmd = m.textInput.Update(msg)
				cmd = newCmd
				return m,cmd
			} else{
				m.textInput.Blur()
				m.Selected[m.cursor] = m.textInput.Value()
				return m, tea.Quit
			}

		}
	}
	switch msg := msg.(type) {

		// Is it a key press?
		case tea.KeyMsg:

			// Cool, what was the actual key pressed?
			switch msg.String() {

			// These keys should exit the program.
			case "ctrl+c", "q":
				os.Exit(1)


			// The "up" and "k" keys move the cursor up
			case "up", "k":
				if m.cursor > 0 {
					m.cursor--
				}

			// The "down" and "j" keys move the cursor down
			case "down", "j":
				if m.cursor < len(m.Choices)-1 {
					m.cursor++
				}

			// The "enter" key and the spacebar (a literal space) toggle
			// the selected state for the item that the cursor is pointing at.
			case "enter", " ":

				for key := range m.Selected {
					delete(m.Selected, key)
				}
				choice := m.Choices[m.cursor]
				m.Selected[m.cursor] = choice
				if choice == m.OtherString && m.Other  {
					m.typing = true
					m.textInput.Focus()
					return m, nil
				}
				return m, tea.Quit
			}


	}

	// Return the updated model to the Bubble Tea runtime for processing.
	// Note that we're not returning a command.
	return m, cmd
}

func (m ShowMenuModel) View() string {
	// The header
	s := m.Prompt + "\n"
	if m.typing{
		return  fmt.Sprintf("Provide a value for OTHER: %s ",m.textInput.View())
	}
	for i, choice := range m.Choices {

		cursor := " "
		if m.cursor == i {
			cursor = ">"
		}

		checked := " "
		if _, ok := m.Selected[i]; ok {
			checked = "x"
		}

		s += fmt.Sprintf("%s [%s] %s\n", cursor, checked, choice)
	}

	// The footer
	// s += "\nPress q to confirm selection.\n"

	// Send the UI for rendering
	return s
}


type ShowMenuMultipleModel struct {
	Defaults            []string          // Default selections
	Prompt              string
	Choices             []string          // Items on the list
	Selected            map[int]string    // Selected items
	cursor              int               // Cursor position
	typing              bool              // If the user is typing
	textInput           textinput.Model   // Text input model
	Other               bool              // Enable 'Other' option
	OtherString         string            // String for 'Other' option
	SelectionLimit      int               // Maximum number of selections allowed
	SelectedDelimiter   string            // Delimiter for selected items in the returned string
}

func ShowMenuMultipleOptions(cliInfo ShowMenuMultipleModel, enableOtherOption interface{}) []string {
	cliInfo.Selected = make(map[int]string)
	cliInfo.textInput = textinput.New()

	if cliInfo.OtherString == "" {
		cliInfo.OtherString = "Other: "
	}
	if cliInfo.Other {
		cliInfo.Choices = append(cliInfo.Choices, cliInfo.OtherString)
	}

	// Mark defaults as selected
	for _, defaultChoice := range cliInfo.Defaults {
		for i, choice := range cliInfo.Choices {
			if choice == defaultChoice {
				cliInfo.Selected[i] = choice
			}
		}
	}

	p := tea.NewProgram(cliInfo)
	if _, err := p.Run(); err != nil {
		fmt.Printf("Alas, there's been an error: %v", err)
		os.Exit(1)
	}

	// Formatting the selected items
	var selectedItems []string
	for _, item := range cliInfo.Selected {
		selectedItems = append(selectedItems, item)
	}

	return selectedItems
}

func (m ShowMenuMultipleModel) Init() tea.Cmd {
	return textinput.Blink
}

func (m ShowMenuMultipleModel) Update(msg tea.Msg) (tea.Model, tea.Cmd) {
	var cmd tea.Cmd
	if m.typing {
		var newCmd tea.Cmd
		m.textInput, newCmd = m.textInput.Update(msg)
		if keyMsg, ok := msg.(tea.KeyMsg); ok && keyMsg.String() == "enter" {
			m.textInput.Blur()
			m.Selected[m.cursor] = m.textInput.Value()
			m.typing = false
			return m, tea.Quit
		}
		cmd = newCmd
		return m, cmd
	}

	switch msg := msg.(type) {
	case tea.KeyMsg:
		switch msg.String() {
		case "ctrl+c", "q":
			os.Exit(1)
		case "up", "k":
			if m.cursor > 0 {
				m.cursor--
			}
		case "down", "j":
			if m.cursor < len(m.Choices)-1 {
				m.cursor++
			}
		case "enter":
			// Toggle selection
			choice := m.Choices[m.cursor]
			if _, ok := m.Selected[m.cursor]; ok {
				delete(m.Selected, m.cursor)
			} else {
				if m.SelectionLimit == 0 || len(m.Selected) < m.SelectionLimit {
					m.Selected[m.cursor] = choice
				}
			}
			if choice == m.OtherString && m.Other {
				m.typing = true
				m.textInput.Focus()
				return m, nil
			}
			return m, nil
		case " ":
			// Return the selected items when spacebar is pressed
			return m, tea.Quit
		}
	}
	return m, cmd
}


func (m ShowMenuMultipleModel) View() string {
	s := m.Prompt + "\n"
	if m.typing {
		return fmt.Sprintf("Provide a value for OTHER: %s", m.textInput.View())
	}
	for i, choice := range m.Choices {
		cursor := " "
		if m.cursor == i {
			cursor = ">"
		}
		checked := " "
		if _, ok := m.Selected[i]; ok {
			checked = "x"
		}
		s += fmt.Sprintf("%s [%s] %s\n", cursor, checked, choice)
	}
	s += fmt.Sprintf("\n Hit SPACE or SPACEBAR when done")
	return s
}
# FileName: \stdio_utils.go 

package utils

import (
	"bufio"
	"bytes"
	"fmt"
	"io"
	"os"
	"os/exec"
	"strings"
)

type TakeVariableArgsStruct struct {
	Prompt    string
	ErrMsg    string
	Default   string
	Delimiter string
}

type TakeVariableArgsResultStruct struct{
	InputString  string
	InputArray   []string
}


func TakeVariableArgs(obj TakeVariableArgsStruct) (TakeVariableArgsResultStruct) {
	var innerScriptArguments []string
	prompt0 := obj.Prompt

	if obj.Delimiter == "" {
		obj.Delimiter = " "
	}
	if obj.Default != "" {
		prompt0 = fmt.Sprintf("%s (Default is %s)", obj.Prompt, obj.Default)
	}
	fmt.Println(prompt0)
	fmt.Println("Enter the arguments to pass to the script (press ENTER to enter another argument, leave blank and press ENTER once done):")
	for {
		var argument string
		fmt.Scanln(&argument)

		if strings.TrimSpace(argument) == "" {
			break
		}

		innerScriptArguments = append(innerScriptArguments, argument)
	}
	input := strings.Join(innerScriptArguments, obj.Delimiter)
	if input == "" && obj.ErrMsg != "" {
		panic(obj.ErrMsg)
	} else if input == "" && obj.Default != "" {
		input = obj.Default
		innerScriptArguments = strings.Split(obj.Default,obj.Delimiter)
	}
	return  TakeVariableArgsResultStruct{
		InputString: input,
		InputArray: innerScriptArguments,
	}

}

type GetInputFromStdinStruct struct {
	Prompt  []string
	ErrMsg  string
	Default string
}

func GetInputFromStdin(obj GetInputFromStdinStruct) string {
	if len(obj.Prompt) == 0 {
		obj.Prompt = []string{"Enter your input: "} // Default value
	} else {
		obj.Prompt[0] += " "
	}

	// Create a new scanner to read from stdin
	scanner := bufio.NewScanner(os.Stdin)

	if obj.Default != "" {
		fmt.Printf("%s (Default is %s) ", obj.Prompt[0], obj.Default)
	} else {
		fmt.Print(obj.Prompt[0])
	}

	// Read the next line of input from stdin
	if !scanner.Scan() && scanner.Err() != nil {
		fmt.Println("Error reading input:", scanner.Err())
		return ""
	}
	input := scanner.Text()

	if input == "" && obj.Default != "" {
		input = obj.Default
	} else if input == "" && obj.ErrMsg != "" {
		panic(obj.ErrMsg)
	}

	return input
}

type ShellCommandOutput struct{}

func (c ShellCommandOutput) Write(p []byte) (int, error) {
	fmt.Println(string(p))
	return len(p), nil
}

// Deprecated: This function will be removed in the next major release. Use RunCommandWithOptions instead.
func RunCommand(command string, args []string) {

	fullCommand := fmt.Sprintf("Running command: %s %s", command, strings.Join(args, " "))
	fmt.Println(fullCommand)
	cmd := exec.Command(command, args...)
	// cmd.Stdout = ShellCommandOutput{}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err := cmd.Run(); err != nil {

		msg := fmt.Sprintf("Could not run command %s %s \n This was the err %s", command, strings.Join(args, " "), err.Error())
		fmt.Println(msg)
		// panic(msg)
	}
}

// Deprecated: This function will be removed in the next major release. Use RunCommandWithOptions instead.
func RunCommandInSpecificDirectory(command string, args []string, targetDir string) {

	fullCommand := fmt.Sprintf("Running command: %s %s", command, strings.Join(args, " "))
	fmt.Println(fullCommand)
	cmd := exec.Command(command, args...)
	cmd.Dir = targetDir
	// cmd.Stdout = ShellCommandOutput{}
	cmd.Stdin = os.Stdin
	cmd.Stdout = os.Stdout
	cmd.Stderr = os.Stderr
	if err := cmd.Run(); err != nil {

		msg := fmt.Sprintf("Could not run command %s %s \n This was the err %s", command, strings.Join(args, " "), err.Error())
		fmt.Println(msg)
		// panic(msg)
	}
}

// Deprecated: This function will be removed in the next major release. Use RunCommandWithOptions instead.
func RunCommandAndGetOutput(command string, args []string) string {

	fullCommand := fmt.Sprintf("Running command: %s %s", command, strings.Join(args, " "))
	fmt.Println(fullCommand)
	output, err := exec.Command(command, args...).Output()
	if err != nil {
		msg := fmt.Sprintf("Could not run command %s %s \n This was the err %s", command, strings.Join(args, " "), err.Error())
		fmt.Println(msg)
		// panic(msg)

	}
	return string(output)
}

// Deprecated: This function will be removed in the next major release. Use RunCommandWithOptions instead.
func RunCommandInSpecifcDirectoryAndGetOutput(command string, args []string, targetDir string) string {

	fullCommand := fmt.Sprintf("Running command: %s %s", command, strings.Join(args, " "))
	fmt.Println(fullCommand)
	cmd := exec.Command(command, args...)
	cmd.Dir = targetDir
	output, err := cmd.Output()
	if err != nil {
		msg := fmt.Sprintf("Could not run command %s %s \n This was the err %s", command, strings.Join(args, " "), err.Error())
		fmt.Println(msg)
		// panic(msg)

	}
	return string(output)
}

type CommandOptions struct {
	CmdObj						   *exec.Cmd
	Self                 *CommandOptions
	Command              string
	Args                 []string
	TargetDir            string
	GetOutput            bool
	PrintOutput          bool
	PrintOutputOnly      bool
	PanicOnError         bool
	NonBlocking          bool
	IsInputFromProgram   bool
}

func (c CommandOptions) EndProcess() ( error) {
	var cmd *exec.Cmd
	if c.Self != nil && c.Self.CmdObj != nil {
			cmd = c.Self.CmdObj
	} else {
			cmd = c.CmdObj
	}
	if cmd != nil{
		return cmd.Process.Kill()

	}
	return nil
}

type DualWriter struct {
	TerminalWriter io.Writer
	Buffer         *bytes.Buffer
}

func (w DualWriter) Write(p []byte) (n int, err error) {
	n, err = w.TerminalWriter.Write(p)
	if err != nil {
		return n, err
	}

	// Write to the buffer as well
	bufferBytes, bufferErr := w.Buffer.Write(p)
	if bufferErr != nil {
		return bufferBytes, bufferErr
	}

	return n, nil
}

func RunCommandWithOptions(options CommandOptions) (string, error) {
	fullCommand := fmt.Sprintf("Running command: %s %s\n", options.Command, strings.Join(options.Args, " "))
	fmt.Println(fullCommand)

	cmd := exec.Command(options.Command, options.Args...)
	if options.Self != nil {
		options.Self.CmdObj = cmd
	}
	if options.TargetDir != "" {
		cmd.Dir = options.TargetDir
	}


	if options.IsInputFromProgram != true{
		cmd.Stdin = os.Stdin
	}

	// Creating buffers and DualWriters for stdout and stderr
	var stdoutBuffer, stderrBuffer bytes.Buffer
	stdoutWriter := DualWriter{TerminalWriter: os.Stdout, Buffer: &stdoutBuffer}
	stderrWriter := DualWriter{TerminalWriter: os.Stderr, Buffer: &stderrBuffer}

	cmd.Stdout = stdoutWriter
	if options.PrintOutput == false {
		cmd.Stdout = &stdoutBuffer
	}
	if options.PrintOutputOnly == true{
		cmd.Stdout = os.Stdout
	}
	cmd.Stderr = stderrWriter


	var err error
	if options.NonBlocking {
		err = cmd.Start() // Non-blocking execution if NonBlocking is true
	} else {
		err = cmd.Run() // Default to blocking execution
	}

	if err != nil {
		// Construct error message
		msg := fmt.Sprintf(
			"Could not run command %s %s\n\nThis was the err: %s \n %s\n\n",
			options.Command,
			strings.Join(options.Args, " "),
			err.Error(),
			fmt.Sprintf("Standard Error: %s\n", stderrBuffer.String()),
		)
		fmt.Println(msg)

		if options.PanicOnError {
			panic(msg)
		}

		return "", err
	}

	if options.GetOutput {
		return stdoutBuffer.String(), nil
	}

	return "", nil
}


# FileName: \string-utils.go 

package utils

import (
	"errors"
	"regexp"
	"strconv"
	"strings"
	"unicode/utf8"

	"github.com/chzyer/readline/runes"
	"github.com/iancoleman/strcase"
)

func Reverse(targetArray []string) []string {
	newArray := make([]string, 0, len(targetArray))
	for i := len(targetArray) - 1; i >= 0; i-- {
		newArray = append(newArray, targetArray[i])
	}
	return newArray
}

func UnicodeUnquote(bs []byte) []byte {
	unicodeEscapeRx := regexp.MustCompile(`\\u[0-9a-fA-F]{4}`)
	return unicodeEscapeRx.ReplaceAllFunc(bs, func(code []byte) []byte {
		rune, _, _, _ := strconv.UnquoteChar(string(code), 0)
		width := runes.Width(rune)
		runeBytes := make([]byte, width)
		utf8.EncodeRune(runeBytes, rune)
		return runeBytes
	})
}



type TruncateStringByRegexOptions struct {
	InputString  string
	RegexPattern string
	Predicate    func(int) bool
}

func TruncateStringByRegex(options TruncateStringByRegexOptions) string {

	regex := regexp.MustCompile(options.RegexPattern)
	matches := regex.FindAllStringIndex(options.InputString, -1)
	currentIndex := 0
	var modifiedString string

	for i := 0; i < len(matches); i++ {
			matchStart, matchEnd := matches[i][0], matches[i][1]

			shouldRemove := true
			if options.Predicate != nil {
				shouldRemove = options.Predicate(i)
			}

			if shouldRemove {
				modifiedString += options.InputString[currentIndex:matchStart]
				currentIndex = matchEnd
			}
	}

	modifiedString += options.InputString[currentIndex:]
	return modifiedString
}

func CreateStringObject(myStr string, entitySuffix string) (CreateStringObjectType, error) {
	if myStr == "" {
		return CreateStringObjectType{}, errors.New("class name is missing or misspelled or the script is having issues finding the class name")
	}

	result := CreateStringObjectType{
		Orig: myStr,
		Prefix: func() string {
			return strings.Split(myStr, entitySuffix)[0]
		},
	}

	result.CamelCase = func(stripSuffix bool, suffix string) string {


		return strcase.ToLowerCamel(grabString(stripSuffix, result))+suffix
	}

	result.Classify = func(stripSuffix bool, suffix string) string {
		return strcase.ToCamel(grabString(stripSuffix, result))+suffix
	}

	result.Capitalize = func(stripSuffix bool, suffix string) string {
		return strings.ToTitle(grabString(stripSuffix, result))+suffix
	}

	result.Dasherize = func(stripSuffix bool, suffix string) string {
		return strcase.ToKebab(grabString(stripSuffix, result))+suffix
	}

	result.Lowercase = func(stripSuffix bool, suffix string) string {
		return strings.ToLower(grabString(stripSuffix, result))+suffix
	}

	result.Uppercase = func(stripSuffix bool, suffix string) string {
		return strings.ToUpper(grabString(stripSuffix, result))+suffix
	}

	result.Snakecase = func(stripSuffix bool, suffix string) string {
		return strcase.ToSnake(grabString(stripSuffix, result))+suffix
	}



	return result, nil
}

func grabString(stripSuffix bool, result CreateStringObjectType) (string) {
	if stripSuffix {
		return  result.Prefix()
	} else {
		return  result.Orig
	}
}

// CreateStringObjectType represents the structure of the string object.
type CreateStringObjectType struct {
	Orig      string
	Prefix    func() string
	CamelCase func(stripSuffix bool, suffix string) string
	Classify  func(stripSuffix bool, suffix string) string
	Capitalize func(stripSuffix bool, suffix string) string
	Dasherize  func(stripSuffix bool, suffix string) string
	Lowercase  func(stripSuffix bool, suffix string) string
	Uppercase  func(stripSuffix bool, suffix string) string
	Snakecase  func(stripSuffix bool, suffix string) string
}

func ContainsAny(s string, substrs []string) bool {
	for _, substr := range substrs {
			if strings.Contains(s, substr) {
					return true
			}
	}
	return false
}

# FileName: \testng_utils.go 

package utils

import (
	"path/filepath"
)

type GetTestNGArgsStruct struct{
	WorkspaceFolder string
	EnvVarsFile string
	TestNGFolder string
	SuiteFile string
	ParamEnv string
}
func GetTestNGArgs(c GetTestNGArgsStruct) GetTestNGArgsStruct {
	c.EnvVarsFile = GetInputFromStdin(
		GetInputFromStdinStruct{
			Prompt: []string{"script where env vars are set for the app to run relative to workspace root"},
			Default: filepath.Join(c.WorkspaceFolder,"ignore\\Local\\testng_e2e_shared.env"),
		},
	)

	c.TestNGFolder = GetInputFromStdin(
		GetInputFromStdinStruct{
			Prompt: []string{"testng app location"},
			Default: filepath.Join(c.WorkspaceFolder,"apps\\testing\\testng"),
		},
	)

	c.SuiteFile = GetInputFromStdin(
		GetInputFromStdinStruct{
			Prompt: []string{"xml suite file needed for testng (this should be relative to the testng folder)"},
			Default: filepath.Join("src\\test\\resources\\tests.xml"),
		},
	)

	cliInfo := ShowMenuModel{
		Prompt: "the environment to test",
		Choices:[]string{"DEV","PREVIEW","PROD"},
		Default:"DEV",
		Other:true,
	}
	c.ParamEnv = ShowMenu(cliInfo,nil)
	return c
}
# FileName: \vscode_settings_utils.go 

package utils

import (
	"encoding/json"
	"fmt"
	"os"
)



type GitCloneSubdirsStruct struct {
	RepoURL    string `json:"repoURL"`
	StagingDir string `json:"stagingDir"`
	Subdirs    string `json:"subdirs"`
	DestDir    string `json:"destDir"`
}

type MiscOptimizeImagesStruct struct {
	Location        string `json:"location"`
	BackupLocation  string `json:"backupLocation"`
	OptimizePercent string `json:"optimizePercent"`
}

type AngularFrontendStruct struct{
	Configurations   []string `json:"configurations"`
}

type WindmillcodeExtensionPack struct {
	TasksToRunOnFolderOpen            []string                 `json:"tasksToRunOnFolderOpen"`
	FlaskBackendDevHelperScript  			string                   `json:"flaskBackendDevHelperScript"`
	FlaskBackendTestHelperScript 			string                   `json:"flaskBackendTestHelperScript"`
	ProxyURLs                    			string                   `json:"proxyURLs"`
	SQLDockerContainerName       			string                   `json:"sqlDockerContainerName"`
	DatabaseName                 			string                   `json:"databaseName"`
	DatabaseOptions              			[]string                 `json:"databaseOptions"`
	OpenAIAPIKey0                			string                   `json:"openAIAPIKey0"`
	OpenAIAPIBase0               			string                   `json:"openAIAPIBase0"`
	LangCodes0                   			string                   `json:"langCodes0"`
	PythonVersion0               			string                   `json:"pythonVersion0"`
	GitCloneSubdirs              			GitCloneSubdirsStruct    `json:"gitCloneSubdirs"`
	MiscOptimizeImages           			MiscOptimizeImagesStruct `json:"miscOptimizeImages"`
	AngularFrontend              			AngularFrontendStruct    `json:"angularFrontend"`
}

type VSCodeSettings struct {
	ExtensionPack WindmillcodeExtensionPack `json:"windmillcode-extension-pack-0"`
}

func GetSettingsJSON(workSpaceFolder string) (VSCodeSettings, error) {
	settingsJSONFilePath := JoinAndConvertPathToOSFormat(workSpaceFolder, "/.vscode/settings.json")
	var settings VSCodeSettings
	content, err := os.ReadFile(settingsJSONFilePath)
	if err != nil {
		fmt.Println("Error reading file:", err.Error())
		return settings, err
	}
	standardJSON, err := RemoveComments(content)
	err = json.Unmarshal([]byte(standardJSON), &settings)
	if err != nil {
		fmt.Println("Error unmarshalling JSON:", err.Error())
		return settings, err
	}
	return settings, nil
}
