# PHP Custom Session Handler

- This script was made for a client that had problems with PHP sessions in WordPress.
- This `Session` class can replace PHP sessions in almost all ways, except for embedding complex objects in this session, but any class that can serialize with PHP's native serialize function will work.

## The `Session Class` defined in `Session.php`

The session files are written to the `wp_content/session` folder, wp_content because it is already writeable in most cases.

```php
// Session.php
<?php
class Session implements ArrayAccess {
    private $container = [];
    private $filePath;
    private $fileName;

    public function __construct() {
        $this->init();
        if (file_exists($this->fileName)) {
            $obj = unserialize(file_get_contents($this->fileName));
            $this->setContainer($obj->getContainer());
        }
    }

    public function setContainer($values) {
        $this->container = $values;
    }

    public function getContainer() {
        return $this->container;
    }

    private function init() {
        $this->filePath = WP_CONTENT_DIR.'/sessions';
        $this->fileName = $this->filePath.'/'.session_id();
        if(is_dir($this->filePath) === false){
            mkdir($dir);
        }
    }

    public function offsetSet($offset, $value) {
        if (is_null($offset)) {
            $this->container[] = $value;
        } else {
            if (strpos($offset,'.') === false) {
                $this->container[$offset] = $value;
            } else {
                $exp = explode('.',$offset);
                if (count($exp) == 3) {
                    $this->container[$exp[0]][$exp[1]][$exp[2]] = $value;
                } elseif(count($exp) == 2) {
                    $this->container[$exp[0]][$exp[1]] = $value;
                }
            }            
        }
    }

    public function offsetExists($offset) {
        if (strpos($offset,'.') === false) {
            return isset($this->container[$offset]);
        } else {
            $exp = explode('.',$offset);
            if (count($exp) == 3) {
                return isset($this->container[$exp[0]][$exp[1]][$exp[2]]);
            } elseif(count($exp) == 2) {
                return isset($this->container[$exp[0]][$exp[1]]);
            }
        }   
    }

    public function offsetUnset($offset) {
        if (strpos($offset,'.') === false) {
            unset($this->container[$offset]);
        } else {
            $exp = explode('.',$offset);
            if (count($exp) == 3) {
                unset($this->container[$exp[0]][$exp[1]][$exp[2]]);
            } elseif(count($exp) == 2) {
                unset($this->container[$exp[0]][$exp[1]]);
            }
        }   
    }

    public function offsetGet($offset) {
        if (strpos($offset,'.') === false) {
            return isset($this->container[$offset]) ? $this->container[$offset] : null;
        } else {
            $exp = explode('.',$offset);
            if (count($exp) == 3) {
                return isset($this->container[$exp[0]][$exp[1]][$exp[2]]) ? $this->container[$exp[0]][$exp[1]][$exp[2]] : null;
            } elseif(count($exp) == 2) {
                return isset($this->container[$exp[0]][$exp[1]]) ? $this->container[$exp[0]][$exp[1]] : null;
            }
        }  
        
    }

    function deleteOldFiles() {
        $files = glob($this->filePath."*");
        $now   = time();

        foreach ($files as $file) {
            if (is_file($file)) {
                if ($now - filemtime($file) >= 60 * 60 * 24) { // 1 days
                    unlink($file);
                }
            }
        }
    }

    function __destruct() {
        $this->deleteOldFiles();
        file_put_contents($this->fileName,serialize($this));
    }
}
```

## Usage

```php
// Instantiate session
$session = new Session();

//Set $session object like it is an array
$session['key'] = $value 

// Set nested array variable, usually it would look like this:
$_SESSION['key']['nest'] = $value;

// But now just dot separate the nesting 3 levels deep max
// Two levels: 
$session['key.nest'] = $value;

// Three levels: Use to look like
$_SESSION['key']['nest']['deeper'] = $value;

// Now it looks like
$session['key.nest.deeper'] = value;