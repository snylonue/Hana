# cuna

Another simple cue parser for Rust with [nom](https://github.com/Geal/nom).  
Supports cue files in UTF-8 and UTF-8 with BOM.  
Current document is not complete and the api is a bit complex, so [rcue](https://github.com/gyng/rcue) or [libcue.rs (though I can't compile this)](https://github.com/mistydemeo/libcue.rs) may be a better choice.

## Usage
cuna 0.5.0 has introduced some incompatible changes, see the [documention](https://docs.rs/cuna) before you upgrade (if you actually wanted).

If you find something not follow the cue standard or confusing, please open an issue.

```rust
use cuna::Cuna;
use cuna::error::Error;

fn main() -> Result<(), Error> {
    let file = "tests/EGOIST - Departures ～あなたにおくるアイの歌～.cue";
    let cue = Cuna::open(file)?;
    assert_eq!(cue.comments[0], "GENRE Pop");
    assert_eq!(cue.header.title, Some(vec!["Departures ～あなたにおくるアイの歌～".to_owned()]));
    assert_eq!(cue[0].name, "EGOIST - Departures ～あなたにおくるアイの歌～.flac");
    assert_eq!(cue.last_track().unwrap().performer(), Some(&vec!["EGOIST".to_owned()]));
    Ok(())
}
```

## [Documention](https://docs.rs/cuna)

## Performance
Here's a benchmark with a 42-line cue file(may be outdated).  
Only test Cuna::from_utf8_with_bom() with i5-7300HQ.
``` 
cuna                    time:   [21.899 us 21.962 us 22.033 us]
                        change: [-1.1745% -0.3960% +0.3489%] (p = 0.31 > 0.05)
                        No change in performance detected.
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe
```

You can run `cargo bench` for more exact data.